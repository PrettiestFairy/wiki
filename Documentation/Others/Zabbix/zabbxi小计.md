## 一. Docker安装zabbix服务端

RedHat8.6

### 1. 创建Docker网卡

```shell
docker network create --opt com.docker.network.bridge.name=docker_zabbix --subnet 172.20.0.0/16 --ip-range 172.20.240.0/20 zabbix_net
```

### 2. 启动MySQL服务

```shell
docker run --network zabbix_net --name zabbix_mysql_server --restart unless-stopped -t -e MYSQL_DATABASE="zabbix" -e MYSQL_USER="zabbix" -e MYSQL_PASSWORD="zabbix_pwd" -e MYSQL_ROOT_PASSWORD="root_pwd" -d mysql:8.0 --character-set-server=utf8mb4 --collation-server=utf8mb4_unicode_ci --default-authentication-plugin=mysql_native_password
```

### 3. 启动Zabbix Java网关

```shell
docker run --name zabbix_java_gateway -t --network zabbix_net --restart unless-stopped -d zabbix/zabbix-java-gateway
```

### 4. 启动Zabbix服务

```shell
docker run --name zabbix_server_mysql -t --network zabbix_net --restart unless-stopped -p 10051:10051 -e DB_SERVER_HOST="zabbix_mysql_server" -e MYSQL_DATABASE="zabbix" -e MYSQL_USER="zabbix" -e MYSQL_PASSWORD="zabbix_pwd" -e MYSQL_ROOT_PASSWORD="root_pwd" -e ZBX_JAVAGATEWAY="zabbix_java_gateway" -d zabbix/zabbix-server-mysql
```

### 5. 启动Zabbix Web界面

```shell
docker run --name zabbix_web_nginx_mysql -t --network zabbix_net --restart unless-stopped -p 91:8080 -e ZBX_SERVER_HOST="zabbix_server_mysql" -e DB_SERVER_HOST="zabbix_mysql_server" -e MYSQL_DATABASE="zabbix" -e MYSQL_USER="zabbix" -e MYSQL_PASSWORD="zabbix_pwd" -e MYSQL_ROOT_PASSWORD="root_pwd" -d zabbix/zabbix-web-nginx-mysql
```

