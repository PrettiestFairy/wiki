# 基于Docker.tgz安装Docker

## 一. 环境说明

| 名称   | 版本   | 下载地址                                                     |
| ------ | ------ | ------------------------------------------------------------ |
| Docker | 23.0.1 | https://download.docker.com/linux/static/stable/x86_64/docker-23.0.1.tgz |

## 二. 部署安装

### 1. 上传文件并进行解压

```shell
tar -zxvf docker-23.0.1.tgz
sudo cp ./docker/* /usr/bin
```

### 2. `docker.service`配置文件

```shell
sudo cp docker.service /etc/systemd/system/
sudo chmod 775 /etc/systemd/system/docker.service
systemctl daemon-reload
```

参考文件:

```ini
[Unit]
Description=Docker Application Container Engine
Documentation=https://docs.docker.com
After=network-online.target firewalld.service
Wants=network-online.target

[Service]
Type=notify
# the default is not to use systemd for cgroups because the delegate issues still
# exists and systemd currently does not support the cgroup feature set required
# for containers run by docker
# ExecStart=/usr/bin/dockerd --selinux-enabled=false --insecure-registry=ip
ExecStart=/usr/bin/dockerd
ExecReload=/bin/kill -s HUP $MAINPID
# Having non-zero Limit*s causes performance problems due to accounting overhead
# in the kernel. We recommend using cgroups to do container-local accounting.
LimitNOFILE=infinity
LimitNPROC=infinity
LimitCORE=infinity
# Uncomment TasksMax if your systemd version supports it.
# Only systemd 226 and above support this version.
#TasksMax=infinity
TimeoutStartSec=0
# set delegate yes so that systemd does not reset the cgroups of docker containers
Delegate=yes
# kill only the docker process, not all processes in the cgroup
KillMode=process
# restart the docker process if it exits prematurely
Restart=on-failure
StartLimitBurst=3
StartLimitInterval=60s

[Install]
WantedBy=multi-user.target
```

## 三. 配置开机启动并运行

```shell
systemctl enable docker.service
systemctl start docker
docker info
```

