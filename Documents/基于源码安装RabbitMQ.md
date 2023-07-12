# 基于源码安装RabbitMQ

## 一.环境说明

| 名称      | 版本号  | 下载地址                                                     |
| --------- | ------- | ------------------------------------------------------------ |
| Erlang    | 25.2.3  | https://github.com/erlang/otp/releases/download/OTP-25.2.3/otp_src_25.2.3.tar.gz |
| RabbitMQ  | 3.11.9  | https://github.com/rabbitmq/rabbitmq-server/releases/download/v3.11.9/rabbitmq-server-generic-unix-3.11.9.tar.xz |
| wxWidgets | 3.2.2.1 | https://github.com/wxWidgets/wxWidgets/releases/download/v3.2.2.1/wxWidgets-3.2.2.1.tar.bz2 |



- 关于RabbitMQ与Erlang版本对应关系. 参见:https://www.rabbitmq.com/which-erlang.html
- Erlang源码安装包下载地址:https://github.com/erlang/otp/releases
- RabbitMQ源码安装包下载地址:https://github.com/rabbitmq/rabbitmq-server/releases
- wxWidgets源码安装包下载地址:https://github.com/wxWidgets/wxWidgets/releases

## 二.安装基础环境

```shell
sudo dnf -y install ncurses gcc gcc-c++ cmake make kernel-devel m4 ncurses-devel openssl-devel gtk2-devel binutils-devel glibc-devel xmlto perl 
```

## 三.安装wxWidgets

```shell
# 解压wxWidgets
tar -xvf wxWidgets-3.2.2.1.tar.bz2
# 进入wxWidgets源码编译目录
cd wxWidgets-3.2.2.1/
# 检查编译环境, 设置安装目录
sudo ./configure --with-regex=builtin --with-gtk --enable-unicode   --disable-shared --prefix=/usr/local/wxWidgets
# 编译
sudo make -j 4 && sudo make install
```

## 四. 安装Erlang

```shell
# 解压erlang到/usr/local/otp_src_25.2.3
sudo tar -zxvf otp_src_25.2.3.tar.gz -C /usr/local/
# 进入erlang源码编译目录
cd /usr/local/otp_src_25.2.3/
# 创建安装目录
sudo mkdir /usr/local/erlang
# 检查编译环境，设置安装目录
sudo ./configure --prefix=/usr/local/erlang
# 编译 & 安装
sudo make && sudo make install
# 将编译安装的erlang加入环境变量，并使之生效（无需进入目录即可执行）
echo 'export PATH=$PATH:/usr/local/erlang/bin' | sudo tee -a /etc/profile
source /etc/profile
# 查看是否安装成功
erl -version
```

## 五.安装RabbitMQ

```shell
# 解压rabbitmq安装包
tar -xvf rabbitmq-server-generic-unix-3.11.9.tar.xz
# 移动rabbitmq目录到/usr/local/rabbitmq
sudo mv rabbitmq_server-3.11.9 /usr/local/rabbitmq
# 将rabbitmq加入环境变量，并使之生效（无需进入目录即可执行）
echo 'export PATH=$PATH:/usr/local/rabbitmq/sbin' | sudo tee -a /etc/profile
source /etc/profile
```

## 六.启动RabbitMQ

```shell
# 后台启动
rabbitmq-server -detached
# 防火墙开放端口
sudo firewall-cmd --add-port=4369/tcp --add-port=5672/tcp --add-port=15672/tcp --add-port=25672/tcp --permanent --zone=public
sudo firewall-cmd --reload
# 添加hosts
echo "127.0.0.1 `hostname`" | sudo tee -a /etc/hosts
# 查看状态
rabbitmqctl status
# 启用管理插件
rabbitmq-plugins enable rabbitmq_management
```

## 七.初始化RabbitMQ用户

```shell
# 添加用户 用户名admin  密码123456 ，密码可以自己指定
rabbitmqctl add_user superadmin Rabbitmq_a12
# 给budget分配vhost
rabbitmqctl set_permissions -p "/" superadmin ".*" ".*" ".*"
# 分配管理权限
rabbitmqctl set_user_tags superadmin administrator
# 重启rabbitmq
rabbitmqctl stop && rabbitmq-server -detached
```
