# Docker

## Docker的基础管理

### 安装docker的准备工作

参考文档 >> https://yeasy.gitbook.io/docker_practice/

DockerHub官网 >> https://hub.docker.com/

1. 升级centos的yum源

```shell
sudo yum update
```

2. 设置yum源

```shell
# 官方源
sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
# 三方源
sudo yum-config-manager --add-repo https://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
```

3. 安装依赖包

```shell
sudo yum install -y yum-utils device-mapper-persistent-data lvm2
```

4. 安装docker

```shell
sudo yum install docker-ce docker-ce-cli containerd.io
```

5. docker镜像加速

参考文档 >> https://www.cnblogs.com/nhdlb/p/12567154.html

```shell
vim /etc/docker/daemon.json

```

修改daemon.json文件

```json
{
	"registry-mirrors": ["https://hub-mirror.c.163.com","https://mirror.baidubce.com"]
}
```

6. docker的启动, 重启, 停止, 查看运行状态

- 启动 >> `systemctl start docker`
- 重启 >> `systemctl restart docker`
- 停止 >> `systemctl stop docker`
- 状态 >> `systemctl status docker`

---

验证是否安装成功:(有Client和Server表示安装启动成功)

```shell
docker version
```

docker镜像

- 搜索

```shell
# 语法: docker search <images>
docker search centos

```

- 下载

```shell
# 语法 docker pull [Operation] [RegistryAddr[:Port]/[Repo[:Tag]]]
docker pull centos:7
```

- 查看

```shell
docker image ls
docker images
```

更详细的查看 >> `docker image ls --no-trunc`
查询某个镜像 >> `docker image inspect <Repository:Tag[Image ID]>`
只查看镜像ID >> `docker images -q`

- 导入导出

导出 >> `docker image save <Repository:Tag[Image ID]> > /opt/<Repository>.tar.gz`
导出 >> `docker image load -i /opt/<Repository>.tar.gz`
改名 >> `docker image tag <Image ID> <Repository:Tag>`

- 删除

删除 >> `docker image rm -f [image ID]` (`-f`表示强制删除)
删除全部 >> `docker image rm -f $(docker image ls -q)` (`docker image ls -q`表示查看全部镜像)

---

```plain
ocker容器以及管理
交互式容器
• 运行容器
运行>>docker container run -it [Image ID]
• 查看容器
查看>>docker container ls
>>Container ID: 容器的唯一ID(默认自动生成)
>>Image: 容器对应的镜像ID
>>Command:启动容器时的命令
>>Created:容器启动时间
>>Status:容器运行状态(Up为正在运行,Exited表示未运行)
>>Ports:容器的端口号
>>Names:容器名称
自定义一个容器名称>>docker container run -it --name='new_centos7' [image ID]
守护式容器
• 运行容器
运行>>docker run -d --name='nginx' [Image ID]
• 查看容器
查看>>docker container inspect [Container Name    或    Container ID]
并验证能否访问
容器的应用场景
• 交互式容器(工具类):开发,测试,临时性的任务等
容器退出交互式shell后自动删除容器>>docker container run -it --name='centos7' --rm [Image ID]
• 守护式容器(服务类):网络维护SSH,
暴露端口运行>>docker run -d -p 8080:80 --name='nginx' [Image ID]
8080端口为自己设置的转发端口号
80为nginx代理的http端口
8080:80映射转发端口号,在任意设备上都可访问
• 对于已有容器的启动,关闭,连接
启动>>docker container start [-i] (交互式) [Container ID]
停止>>docker container stop [Container ID]
连接>>docker container attach [交互式镜像Container ID]
>>docker container exec -it [交互式子进程Container ID] /bin/bash    在已有工作容器中生成子进程登录,可做调试,也不会影响原有容器的运行
容器的网络访问
• 指定映射(默认tcp)
>>docker run -d -p 8080:80 --name='nginx' [Image ID]
>>docker run -d -p 192.168.1.1:8081:80 --name='nginx' [Image ID] 对应网段固定端口号
• 随机映射(默认tcp)
>>docker run -d -p 80 --name='nginx' [Image ID]
>>docker run -d -p 192.168.1.1::80 --name='nginx' [Iamge ID]对应网段端口号随机
容器的其他管理
只看ID>>docker Image ls -aq    docker container ls -aq 等
看logs>>docker container logs [Container ID]  显示日志信息
>docker container logs -t [-tf] [Container ID]  显示更详细的日志信息(时间戳)
总结:
1,镜像类
docker image
search查(是否有docker镜像)
pull下载
ls查询(已下载的)
inspect详细信息
rm删除
save导出
load导入
2,容器类
docker container
run运行
start启动
stop停止
restart重启
kill杀死
inspect详细信息
exec用/bin/bash进入容器
attach进入容器
ls查看
top查看
logs查看日志

```

## Docker的数据卷

```plain
守护式服务(持久化存储)
• 1.拷贝文件(docker container cp)
宿主机-->docker容器:docker container cp [host file] [container name]:[container address]
docker容器-->宿主机:docker container cp [container name]:[container file] [host address]
• 2.Volume实现宿主机和容器的数据共享(以nginx为例)
数据卷的映射:
建立一个宿主机和docker容器的映射文件夹
>>mkdir -p /opt/dockernginx
启动时映射(宿主机上修改文件docker容器文件夹默认同步)
>>docker run -d -p [host port]:[container port] --name='[container name]' -v /opt/dockernginx[host fileaddr]:/usr/share/nginx/html[container fileaddr]
数据卷容器 
1. 宿主机
mkdir -p /opt/volume/a /opt/volume/b
touch /opt/volume/a/a.txt /opt/volume/b/b.txt
2. 启用数据卷容器:映射到docker容器中(centos:7)
docker run -it --name='nginx_volumes' -v /opt/volume/a:/opt/a -v /opt/volume/b:/opt/b centos:7 /bin/bash
3. 使用数据卷容器
docker run -d -p 8081:80 --volumes-from nginx_volumes --name='nginx_8081' nginx
docker run -d -p 8082:80 --volumes-from nginx_volumes --name='nginx_8082' nginx
作用: 在集中管理集群中,大批量的容器都需要挂载相同数据卷,可以采用数据卷容器进行统一管理
```

## Docker的镜像制作

```plain
1.容器优化
使用对应版本的centos做yum源
参考文档>>https://www.cnblogs.com/longloved/p/14918224.html
在docker镜像中安装
>>yum install -y openssh*
>>yum install -y openssh-server
启动ssh服务
>>/etc/init.d/sshd start
停止>>/etc/init.d/sshd stop
重启>>/etc/init.d/sshd restart
宿主机用ssh连接docker
>>ssh [eth_docker IP]
2.基于容器的镜像制作
制作容器
>>docker commit [container name] [commit image name]
启动容器的父命令=/usr/sbin/sshd -D
>>docker run -d --name=[container name] [image name:Tag] /usr/sbin/sshd -D
添加公网port
>>docker run -d --name=ssh_8022 -p 8022:22  [centos/centos6.10_ssh:v1]  /usr/sbin/sshd -D

构建企业镜像(基于Centos6.9)
新建数据卷宿主机对应的目录
>>mkdir -p /opt/volume/mysql /opt/volume/html

启用基础镜像容器
运行>>docker run -it --name=centos_BBS -v /opt/volume/mysql:/var/lib/mysql -v /opt/volume/html:/vat/www/html centos:6.9 /bin/bash

容器yum换源
新建备份文件夹>>mkdir /etc/yum.repos.d/back
移动源到文件夹>>mv /etc/yum.repos.d/*.repo /etc/yum.repos.d/back
配置yum源文件>>echo -e "[ftp]/nname = ftpbase/nbaseurl = ftp://172.17.0.1/centos6.9/nenabled = 1/ngpcheck = 0" > /etc/yum.repos.d/ftp_centos6.9.repo
配置yum源文件>>cat >/etc/yum.repo.d/ftp_centos6.9 <<EOF
[ftp]
name = ftpbase
baseurl = ftp://172.17.0.1/centos6.9
enabled = 1
gpgcheck = 0
EOF
清除yum缓存>>yum clean all
重建yum缓存>>yum makecache

安装软件并进行初始化
设置密码
>>passwd
>>echo "root" | passwd root --stdin
安装ssh
>>yum install -y openssh*
>>yum install -y openssh-server
安装Apache
>>yum install -y httpd
安装mysql以及mysql-server
>>yum install -y mysql
>>yum install -y mysql-server
安装PHP以及PHP-mysql
>>yum install -y php
>>yum install -y php-mysql
初始化ssh
>>/etc/init.d/sshd start
初始化mysql
>>/etc/init.d/mysqld start
mysql的用户授权等操作
用户授权>>grant all on *.* to root@'%' identified by 'root';
	>>grant all on *.* to discuss@'%' identified by '123';
建库>> create database discuss charset utf8;
初始化Apache
>>/etc/init.d/httpd start

制作第一版镜像
>>docker commit [container nameN/ID] [Images nameNO1:Tag]

上传文件到宿主机/opt/volume/html下解压
discuz下载地址>>https://longloved.lanzoui.com/iI6jEql8bde 密码:discuz

启动第一版镜像
>>docker run -it -p 8080:80 --name='BBS' -v /opt/volume/mysql:/var/lib/mysql/ -v /opt/volume/html:/var/www/html [Image nameNO1:Tag]
重新启动mysql和Apache
>>/etc/init.d/httpd start
>>/etc/init.d/mysqld start
安装discuz
浏览器访问localhost:[port]/[解压后的文件名]
**直接解压到html/目录下的访问localhost:[port]

制作第二版镜像
>>docker commit [container name/ID] [Image nameNO2:Tag]

宿主机创建启动脚本程序
cat>/opt/volume/sh/bbs.sh<<EOF
#!/bin/bash
/etc/init.d/httpd start
/etc/init.d/mysqld start 
/usr/sbin/sshd -D

脚本设置权限为777
chmod 777 /opt/volume/sh/bbs.sh

启动第二版镜像
docker run -d --name=run_bbs -p 10022:22 -p 10080:80 -p 13306:3306 -v /opt/volume/mysql/:/var/lib/mysql -v /opt/volume/html/:/var/www/html -v /opt/volume/sh/:/opt/volume/sh  centos6.9_bbs /opt/volume/sh/bbs.sh

```

## Dockerfile的基本使用

```plain
1.dockerfile 的基本使用
>>mkdir -p /opt/dockerfile/centos6.9_file
>>vi /opt/docekrfile/centos6.9_file/dockerfile

2.dockerfile文件
FROM 基础镜像
必须以FROM开头
	#FROM 语法
	#FROM REPOSITORY:Tag  一般
	#FROM REPOSITORY@ImageID  Hash校验
RUN 构建镜像过程中用到的命令
EXPOSE 暴露端口
CMD 使用镜像启动容器用到的命令
ADD 添加文件,tar格式的直接解压,也可以更url
COPY 复制文件
ENV 添加变量
	用法:
		ENV CODE_DIR /var/www/html
		COPY project.tar.gz ${CODE_DIR}
VOLUME 数据卷挂载
ENTRYPOINT 在容器启动时,第一进程被手工输入的命令替换的,防止容器秒开秒关


3.构建镜像  
>>docker image build -t "image name" /dockerfile的目录
* 是目录不是文件

4.Dockerfile命令

```

- 1

```dockerfile
# Centos6.9_SSHDv1.0
# dockerfile
FROM docker.io/centos:6.9
RUN mkdir -p /etc/yum.repos.d/back && mv /etc/yum.repos.d/*.repo /etc/yum.repos.d/back && echo -e "[ftp]\nname = ftpbase\nbaseurl = ftp://172.17.0.1/centos6.9\nenabled = 1\ngpgcheck = 0" > /etc/yum.repos.d/ftp_centos6.9.repo && yum clean all && yum makecache && yum install -y openssh-server
RUN /etc/init.d/sshd start && echo "root" | passwd root --stdin
EXPOSE 22
CMD ["/usr/sbin/sshd","-D"]

```

- 2

```dockerfile
# Centos6.9_SSHD_LAMP
FROM docker.io/centos:6.9
RUN mkdir /etc/yum.repos.d/bak && mv /etc/yum.repos.d/*.repo /etc/yum.repos.d/bak && echo -e "[ftp]\nname = ftpbase\nbaseurl = ftp://172.17.0.1/centos6.9\nenabled = 1\ngpgcheck = 0" > /etc/yum.repos.d/ftp_centos6.9.repo && yum clean all && yum makecache && yum install -y openssh-server httpd mysql mysql-server php php-mysql
RUN echo "root" | passwd root --stdin && /etc/init.d/sshd start && /etc/init.d/mysqld start && /etc/init.d/httpd start 
# RUN "mysql -e "grant all on *.* to root@'%' identified by 'root'; grant all on *.* to discuss@'%' identified by '123'; create database discuss charset utf8;"
COPY index.php /tmp
COPY init.sh /
ADD bbs.tar.gz /var/www/html
# ADD https://vip.d0.baidupan.com/file/?AGZaZA8+BTQHDgc/V2JdMVplU2sHPlE6BW5XNAFzVCEEfAckWSIDMVZpVG4DMlwCBzRVNFMxAzUBMFtlUzZabgAwWj4PagV3BzcHIlc+XWpaJlMmByhRbgUwV2EBb1RkBCsHYFkzA3hWMFQzA3hcOQduVWVTbgMxAWdbZFM9WmgANFo7D2sFMgc0B2RXYF1sWjVTYAc6UTUFZlcyAThUYgQzB2BZMwNgVmRUZgNnXHMHKVUoU3gDJwFzWy5TalopAGlaaQ9vBWAHYQdmVzRdbFo5U2MHflEnBWtXPAE4VDMEOQdhWTIDZVY2VDoDYFxuB2VVaFM0Ay8BIFsoU39aZgAxWiMPdAU3Bz0HJlc+XW1aNlNkB21RYgUzV2cBbVRmBDEHdllxAydWd1Q/A2ZcawdvVWVTPQMwATBbbVM+WmgAJlp4DzsFIQdsB2NXMV1uWi5TZAdpUWEFLFdjAXNUZAQ9B2k= /tmp
EXPOSE 22
EXPOSE 80
EXPOSE 3306
CMD ["/bin/bash","/init.sh"]
```

- 3

```dockerfile
# dockerfile
From docker.io/centos:6.9
RUN mkdir /etc/yum.repos.d/bak && mv /etc/yum.repos.d/*.repo /etc/yum.repos.d/bak && echo -e "[ftp]\nname=ftpbase\nbaseurl=ftp://172.17.0.1/centos6.9\nenabled=1\ngpgcheck=0" > /etc/yum.repos.d/ftp.repo && yum clean all && yum makecache && yum install -y openssh-server httpd mysql mysql-server php php-mysql
RUN echo "root" | passwd root --stdin && /etc/init.d/sshd start && /etc/init.d/mysqld start && /etc/init.d/httpd start
# RUN mysql -e "grant all on *.* to root@'%' identified by 'root'; grant all on *.* to discuss@'%' identified by '123456'; create database discuss charset utf8;"
ADD discuss.tar.gz /var/www/html
COPY index.php /var/www/html
COPY init.sh /
ADD https://vip.d0.baidupan.com/file/?AGZaZA8+BTQHDgc/V2JdMVplU2sHPlE6BW5XNAFzVCEEfAckWSIDMVZpVG4DMlwCBzRVNFMxAzUBMFtlUzZabgAwWj4PagV3BzcHIlc+XWpaJlMmByhRbgUwV2EBb1RkBCsHYFkzA3hWMFQzA3hcOQduVWVTbgMxAWdbZFM9WmgANFo7D2sFMgc0B2RXYF1sWjVTYAc6UTUFZlcyAThUYgQzB2BZMwNgVmRUZgNnXHMHKVUoU3gDJwFzWy5TalopAGlaaQ9vBWAHYQdmVzRdbFo5U2MHflEnBWtXPAE4VDMEOQdhWTIDZVY2VDoDYFxuB2VVaFM0Ay8BIFsoU39aZgAxWiMPdAU3Bz0HJlc+XW1aNlNkB21RYgUzV2cBbVRmBDEHdllxAydWd1Q/A2ZcawdvVWVTPQMwATBbbVM+WmgAJlp4DzsFIQdsB2NXMV1uWi5TZAdpUWEFLFdjAXNUZAQ9B2k= /opt
EXPOSE 22
EXPOSE 80
EXPOSE 3306
CMD ["/bin/bash", "/init.sh"]

# docker run -d -P --name='bbs' -v /opt/volume/mysql:/var/lib/mysql -v /opt/volume/html:/var/www/html bbs

```

