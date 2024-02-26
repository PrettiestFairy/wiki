# Kubernetes集群搭建

## 一. 集群说明

| 节点             | IP              |
| ---------------- | --------------- |
| Master(RHEL8.8)  | 192.168.254.151 |
| Slave01(RHEL8.8) | 192.168.254.152 |
| Slave02(RHEL8.8) | 192.168.254.153 |

## 二. 系统初始化

### 1. 设置系统主机名以及Hosts文件的相互解析

- 设置主机名

  ```shell
  # 分别在三台服务器上执行
  hostnamectl set-hostname vm1
  hostnamectl set-hostname vm2
  hostnamectl set-hostname vm3
  ```

- 配置hosts

  > 192.168.254.151 vm1
  > 192.168.254.152 vm2
  > 192.168.254.153 vm3

  ```shell
  # 分发hosts
  scp /etc/hosts root@vm2:/etc/hosts
  scp /etc/hosts root@vm2:/etc/hosts
  ```

### 2. 更新软件包

```shell
dnf update
# 安装软件
dnf install -y conntrack ipvsadm ipset jq iptables curl sysstat libseccomp wget vim net-tools git chrony
```

### 3. 设置防火墙为iptables并设置空规则

```shell
systemctl stop firewalld && systemctl disable firewalld
dnf -y install iptables-services && systemctl start iptables && systemctl enable iptables && iptables -F && service iptables save
```

### 4. 关闭Swap和SELinux

```shell
swapoff -a && sed -i '/ swap / s/^\(.*\)$/#\1/g' /etc/fstab
setenforce 0 && sed -i 's/^SELINUX=.*/SELINUX=disabled/' /etc/selinux/config
```

### 5. 调整内核参数(K8s)

```shell
cat > kubernetes.conf <<EOF
net.bridge.bridge-nf-call-iptables=1
net.bridge.bridge-nf-call-ip6tables=1
net.ipv4.ip_forward=1
net.ipv4.tcp_tw_recycle=0
vm.swappiness=0 # 禁止使用 swap 空间，只有当系统 OOM 时才允许使用它
vm.overcommit_memory=1 # 不检查物理内存是否够用
vm.panic_on_oom=0 # 开启 OOM
fs.inotify.max_user_instances=8192
fs.inotify.max_user_watches=1048576
fs.file-max=52706963
fs.nr_open=52706963
net.ipv6.conf.all.disable_ipv6=1
net.netfilter.nf_conntrack_max=2310720
EOF
cp kubernetes.conf /etc/sysctl.d/kubernetes.conf
sysctl -p /etc/sysctl.d/kubernetes.conf
```

### 6. 设置rsyslogd和systemd journald

```shell
mkdir /var/log/journal # 持久化保存日志的目录
mkdir /etc/systemd/journald.conf.d
cat > /etc/systemd/journald.conf.d/99-prophet.conf <<EOF
[Journal]
# 持久化保存到磁盘
Storage=persistent
# 压缩历史日志
Compress=yes
SyncIntervalSec=5m
RateLimitInterval=30s
RateLimitBurst=1000
# 最大占用空间 10G
SystemMaxUse=10G
# 单日志文件最大 200M
SystemMaxFileSize=200M
# 日志保存时间 2 周
MaxRetentionSec=2week
# 不将日志转发到 syslog
ForwardToSyslog=no
EOF
systemctl restart systemd-journald
```

## 三. 部署安装

### 1. kube-proxy开启ipvs的前置条件

```shell
modprobe br_netfilter
cat > /etc/sysconfig/modules/ipvs.modules <<EOF
#!/bin/bash
modprobe -- ip_vs
modprobe -- ip_vs_rr
modprobe -- ip_vs_wrr
modprobe -- ip_vs_sh
modprobe -- nf_conntrack
EOF
chmod 755 /etc/sysconfig/modules/ipvs.modules && bash /etc/sysconfig/modules/ipvs.modules && lsmod | grep -e ip_vs -e nf_conntrack
sysctl -p /etc/sysctl.d/kubernetes.conf
```

### 2. 安装Docker

```shell
# 卸载podman冲突包
yum erase podman buildah -y
# 安装yum-utils和dnf-utils
dnf install -y yum-utils dnf-utils
yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
dnf install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin -y
# 创建/etc/docker 目录
mkdir /etc/docker
# 配置daemon.json
cat > /etc/docker/daemon.json <<EOF
{
	"exec-opts": ["native.cgroupdriver=systemd"],
	"log-driver": "json-file",
	"log-opts": {
	"max-size": "100m"
	}
}
EOF
mkdir -p /etc/systemd/system/docker.service.d
# 重启docker服务
systemctl daemon-reload && systemctl restart docker && systemctl enable docker
```

### 3. 安装Kubeadm（主从配置）

```shell
cat <<EOF > /etc/yum.repos.d/kubernetes.repo
[kubernetes]
name=Kubernetes
baseurl=https://mirrors.aliyun.com/kubernetes/yum/repos/kubernetes-el7-x86_64/
enabled=1
gpgcheck=1
repo_gpgcheck=1
gpgkey=https://mirrors.aliyun.com/kubernetes/yum/doc/yum-key.gpg https://mirrors.aliyun.com/kubernetes/yum/doc/rpm-package-key.gpg
EOF
yum install -y kubelet kubeadm kubectl
systemctl enable kubelet && systemctl start kubelet
```

- 初始化主节点

  ```shell
  kubeadm config print init-defaults > kubeadm-config.yaml
  
  localAPIEndpoint:
  advertiseAddress: 192.168.66.10
  kubernetesVersion: v1.15.1
  networking:
  podSubnet: "10.244.0.0/16"
  serviceSubnet: 10.96.0.0/12
  ---
  apiVersion: kubeproxy.config.k8s.io/v1alpha1
  kind: KubeProxyConfiguration
  featureGates:
  SupportIPVSProxyMode: true
  mode: ipvs
  kubeadm init --config=kubeadm-config.yaml --upload-certs | tee kubeadm-init.log
  ```

- 加入主节点以及其余工作节点

- 部署网络

  ```shell
  kubectl apply -f https://raw.githubusercontent.com/flannel-io/flannel/master/Documentation/kube-flannel.yml
  ```

