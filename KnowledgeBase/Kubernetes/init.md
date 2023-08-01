# 服务器初始化

> 软件安装:
>
> dnf install -y vim net-tools yum-utils dnf-utils nc iproute-tc wget bash-completion

## 一 初始化 hostname 并配置 hosts

> vm1,vm2,vm3三台主机root用户运行

初始化hostname

```shell
hostnamectl set-hostname vm1
hostnamectl set-hostname vm2
hostnamectl set-hostname vm3
```

配置hosts文件

```text
# 配置hosts
192.168.254.151 vm1
192.168.254.152 vm2
192.168.254.153 vm3
```

## 二 分发 hosts 文件

> vm1主机root用户运行

```shell
scp /etc/hosts root@vm2:/etc/hosts
scp /etc/hosts root@vm3:/etc/hosts
```

## 三 关闭 SElinux 和 Swap

> vm1,vm2,vm3三台主机root用户运行

```shell
swapoff -a && sed -i '/ swap / s/^\(.*\)$/#\1/g' /etc/fstab
setenforce 0 && sed -i 's/^SELINUX=.*/SELINUX=disabled/' /etc/selinux/config
```

## 四 设置防火墙为 iptables 并设置空规则

> vm1,vm2,vm3三台主机root用户运行

```shell
systemctl stop firewalld && systemctl disable firewalld
dnf -y install iptables-services && systemctl start iptables && systemctl enable iptables && iptables -F && service iptables save
```

## 五 安装 Docker

> vm1,vm2,vm3三台主机root用户运行

```shell
# 卸载podman冲突包
yum erase podman buildah -y
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
	"max-size": "10m"
	}
}
EOF
mkdir -p /etc/systemd/system/docker.service.d
# 重启docker服务
systemctl daemon-reload && systemctl restart docker && systemctl enable docker
```

## 六 调整内核参数(K8s)

> vm1,vm2,vm3三台主机root用户运行

```shell
cat > kubernetes.conf <<EOF
net.bridge.bridge-nf-call-ip6tables=1
net.bridge.bridge-nf-call-iptables=1
net.ipv4.ip_forward=1
#禁止使用 swap 空间，只有当系统 OOM 时才允许使用它
vm.swappiness=0
#不检查物理内存是否够用
vm.overcommit_memory=1
#开启 OOM
vm.panic_on_oom=0
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

在所有节点上加载以下内核模块

```shell
sudo tee /etc/modules-load.d/containerd.conf <<EOF
overlay
br_netfilter
EOF
sudo modprobe overlay
sudo modprobe br_netfilter
```

## 七 设置 rsyslogd 和 systemd journald

>vm1,vm2,vm3三台主机root用户运行

```shell
# 持久化保存日志的目录
mkdir /var/log/journal
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

## 八 kube-proxy 开启 ipvs 的前置条件

> vm1,vm2,vm3三台主机root用户运行

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

sudo sysctl --system
```

## 九 安装 kubeadm, kubelet 和 kubectl

> vm1,vm2,vm3三台主机root用户运行(vm2,vm3 不用安装 kubectl)

```shell
cat <<EOF | sudo tee /etc/yum.repos.d/kubernetes.repo
[kubernetes]
name=Kubernetes
baseurl=https://packages.cloud.google.com/yum/repos/kubernetes-el7-x86_64
enabled=1
gpgcheck=1
gpgkey=https://packages.cloud.google.com/yum/doc/rpm-package-key.gpg
exclude=kubelet kubeadm kubectl
EOF
sudo yum install -y kubelet kubeadm kubectl --disableexcludes=kubernetes
sudo systemctl enable --now kubelet
```

### 防止报错大法

> vm1,vm2,vm3三台主机root用户运行

```shell
sudo rm /etc/containerd/config.toml
# 可选
# 生成 containerd 的默认配置文件
containerd config default > /etc/containerd/config.toml 
# 换源(可选)
sed -i "s#registry.k8s.io/pause#registry.aliyuncs.com/google_containers/pause#g" /etc/containerd/config.toml
# 重新加载配置文件
systemctl daemon-reload 
# 重启所有有关6443端口的服务 
systemctl restart containerd.service 
systemctl restart docker
systemctl restart kubelet
```

## 十 Master 节点初始化

> vm1主机root用户运行

kubeadm-init.yaml

```yaml
apiVersion: kubeadm.k8s.io/v1beta3
bootstrapTokens:
  - groups:
      - system:bootstrappers:kubeadm:default-node-token
    token: abcdef.0123456789abcdef
    ttl: 24h0m0s
    usages:
      - signing
      - authentication
kind: InitConfiguration
localAPIEndpoint:
  # 主机 IP
  advertiseAddress: 192.168.254.151
  bindPort: 6443
nodeRegistration:
  criSocket: unix:///var/run/containerd/containerd.sock
  imagePullPolicy: IfNotPresent
  name: master
  taints: null
---
apiServer:
  timeoutForControlPlane: 4m0s
apiVersion: kubeadm.k8s.io/v1beta3
certificatesDir: /etc/kubernetes/pki
clusterName: kubernetes
controllerManager: { }
dns: { }
etcd:
  local:
    dataDir: /var/lib/etcd
imageRepository: registry.k8s.io
kind: ClusterConfiguration
kubernetesVersion: 1.27.4
networking:
  dnsDomain: cluster.local
  # 默认地址
  serviceSubnet: 10.96.0.0/12
  # Pod network 和 flannel的一样
  podSubnet: 10.244.0.0/16
scheduler: { }
---
apiVersion: kubeproxy.config.k8s.io/v1alpha1
kind: KubeProxyConfiguration
mode: ipvs

```

初始化

```shell
kubeadm init --config kubeadm-init.yaml
# 节点加入
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
export KUBECONFIG=/etc/kubernetes/admin.conf
```

## 十一 部署 Flannel 网络

kube-flannel.yaml

```yaml
---
kind: Namespace
apiVersion: v1
metadata:
  name: kube-flannel
  labels:
    k8s-app: flannel
    pod-security.kubernetes.io/enforce: privileged
---
kind: ClusterRole
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  labels:
    k8s-app: flannel
  name: flannel
rules:
  - apiGroups:
      - ""
    resources:
      - pods
    verbs:
      - get
  - apiGroups:
      - ""
    resources:
      - nodes
    verbs:
      - get
      - list
      - watch
  - apiGroups:
      - ""
    resources:
      - nodes/status
    verbs:
      - patch
  - apiGroups:
      - networking.k8s.io
    resources:
      - clustercidrs
    verbs:
      - list
      - watch
---
kind: ClusterRoleBinding
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  labels:
    k8s-app: flannel
  name: flannel
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: flannel
subjects:
  - kind: ServiceAccount
    name: flannel
    namespace: kube-flannel
---
apiVersion: v1
kind: ServiceAccount
metadata:
  labels:
    k8s-app: flannel
  name: flannel
  namespace: kube-flannel
---
kind: ConfigMap
apiVersion: v1
metadata:
  name: kube-flannel-cfg
  namespace: kube-flannel
  labels:
    tier: node
    k8s-app: flannel
    app: flannel
data:
  cni-conf.json: |
    {
      "name": "cbr0",
      "cniVersion": "0.3.1",
      "plugins": [
        {
          "type": "flannel",
          "delegate": {
            "hairpinMode": true,
            "isDefaultGateway": true
          }
        },
        {
          "type": "portmap",
          "capabilities": {
            "portMappings": true
          }
        }
      ]
    }
  # 网卡子网和 Pod 的子网一样
  net-conf.json: |
    {
      "Network": "10.244.0.0/16",
      "Backend": {
        "Type": "vxlan"
      }
    }
---
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: kube-flannel-ds
  namespace: kube-flannel
  labels:
    tier: node
    app: flannel
    k8s-app: flannel
spec:
  selector:
    matchLabels:
      app: flannel
  template:
    metadata:
      labels:
        tier: node
        app: flannel
    spec:
      affinity:
        nodeAffinity:
          requiredDuringSchedulingIgnoredDuringExecution:
            nodeSelectorTerms:
              - matchExpressions:
                  - key: kubernetes.io/os
                    operator: In
                    values:
                      - linux
      hostNetwork: true
      priorityClassName: system-node-critical
      tolerations:
        - operator: Exists
          effect: NoSchedule
      serviceAccountName: flannel
      initContainers:
        - name: install-cni-plugin
          image: docker.io/flannel/flannel-cni-plugin:v1.2.0
          command:
            - cp
          args:
            - -f
            - /flannel
            - /opt/cni/bin/flannel
          volumeMounts:
            - name: cni-plugin
              mountPath: /opt/cni/bin
        - name: install-cni
          image: docker.io/flannel/flannel:v0.22.1
          command:
            - cp
          args:
            - -f
            - /etc/kube-flannel/cni-conf.json
            - /etc/cni/net.d/10-flannel.conflist
          volumeMounts:
            - name: cni
              mountPath: /etc/cni/net.d
            - name: flannel-cfg
              mountPath: /etc/kube-flannel/
      containers:
        - name: kube-flannel
          image: docker.io/flannel/flannel:v0.22.1
          command:
            - /opt/bin/flanneld
          args:
            - --ip-masq
            - --kube-subnet-mgr
          resources:
            requests:
              cpu: "100m"
              memory: "50Mi"
          securityContext:
            privileged: false
            capabilities:
              add: [ "NET_ADMIN", "NET_RAW" ]
          env:
            - name: POD_NAME
              valueFrom:
                fieldRef:
                  fieldPath: metadata.name
            - name: POD_NAMESPACE
              valueFrom:
                fieldRef:
                  fieldPath: metadata.namespace
            - name: EVENT_QUEUE_DEPTH
              value: "5000"
          volumeMounts:
            - name: run
              mountPath: /run/flannel
            - name: flannel-cfg
              mountPath: /etc/kube-flannel/
            - name: xtables-lock
              mountPath: /run/xtables.lock
      volumes:
        - name: run
          hostPath:
            path: /run/flannel
        - name: cni-plugin
          hostPath:
            path: /opt/cni/bin
        - name: cni
          hostPath:
            path: /etc/cni/net.d
        - name: flannel-cfg
          configMap:
            name: kube-flannel-cfg
        - name: xtables-lock
          hostPath:
            path: /run/xtables.lock
            type: FileOrCreate


```

## 十二 Node 节点加入集群

> vm2,vm3三台主机root用户运行

```shell
# 每个人的不一样
kubeadm join 192.168.254.151:6443 --token abcdef.0123456789abcdef \
        --discovery-token-ca-cert-hash sha256:01515d4b4f46c2a975af475465f32da64ec105167946d135811aca38a2eb46a8
```

## 十三 附录

### crictl.yaml

`/etc/crictl.yaml`

```yaml
runtime-endpoint: "unix:///var/run/containerd/containerd.sock"
image-endpoint: ""
timeout: 0
debug: false
pull-image-on-create: false
disable-pull-on-run: false
```

### daemon.json

`/etc/docker/daemon.json`

```json
{
"exec-opts": ["native.cgroupdriver=systemd"],
"log-driver": "json-file",
"log-opts": {
"max-size": "10m"
}
}
```

