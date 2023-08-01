[toc]

# Ubuntu服务器系统优化

## 一. 禁用内核更新

```shell
sudo apt-mark hold linux-image-generic linux-headers-generic
```

修改文件

```shell
sudo vim /etc/apt/apt.conf.d/10periodic
sudo vim /etc/apt/apt.conf.d/20auto-upgrades
```

## 二. apt换源

参照[清华镜像中心帮助文档](https://mirrors.tuna.tsinghua.edu.cn/help/ubuntu/)

## 三. 静态IP配置

路径: `/etc/netplan/`中

参照:

```yaml
network:
  ethernets:
    ens33:
      addresses:
      - 192.168.254.128/24
      gateway4: 192.168.254.10
      nameservers:
        addresses:
        - 192.168.254.10
        search:
        - localhost
  version: 2
```

应用配置文件`sudo netplan apply`

