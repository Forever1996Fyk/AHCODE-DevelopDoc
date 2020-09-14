# 搭建MySQL集群

准备3台带有MySQL服务的服务器, MySQL最好版本一致。本项目中使用 `MySQL 8.0.21`

## 1. 使用docker-compose启动容器

我们使用`docker-compose.yml`启动容器, 并且挂载MySQL容器的数据卷以及配置文件

### 1.1 编写`docker-compose.yml`

```yaml
version: '3.1'
services:
  db:
    image: mysql:8.0.21
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: 123456
    command:
      --default-authentication-plugin=mysql_native_password
      --character-set-server=utf8mb4
      --collation-server=utf8mb4_general_ci
      --explicit_defaults_for_timestamp=true
      --lower_case_table_names=1
    ports:
      - 3306:3306
    volumes:
      - /usr/etc/docker-compose/mysql/data:/var/lib/mysql # 映射mysql容器中的数据卷到宿主机上
      - /usr/etc/docker-compose/mysql/my.cnf:/etc/mysql/my.cnf # 映射mysql容器中的配置文件到宿主机上
      - /usr/etc/docker-compose/mysql/log:/var/log/mysql
```

### 1.2 在宿主机中准备`MySQL 8.0.21`的主从库配置文件`my.cnf`:

```conf

# Copyright (c) 2017, Oracle and/or its affiliates. All rights reserved.
#
# This program is free software; you can redistribute it and/or modify
# it under the terms of the GNU General Public License as published by
# the Free Software Foundation; version 2 of the License.
#
# This program is distributed in the hope that it will be useful,
# but WITHOUT ANY WARRANTY; without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
# GNU General Public License for more details.
#
# You should have received a copy of the GNU General Public License
# along with this program; if not, write to the Free Software
# Foundation, Inc., 51 Franklin St, Fifth Floor, Boston, MA  02110-1301 USA

#
# The MySQL  Server configuration file.
#
# For explanations see
# http://dev.mysql.com/doc/mysql/en/server-system-variables.html

[mysqld]
pid-file        = /var/run/mysqld/mysqld.pid
socket          = /var/run/mysqld/mysqld.sock
datadir         = /var/lib/mysql
secure-file-priv= NULL
# Disabling symbolic-links is recommended to prevent assorted security risks
symbolic-links=0

# Custom config should go here
!includedir /etc/mysql/conf.d/

# 从库配置
server-id=192168100110 # 注意server-id必须唯一, 这里取的是ip地址
relay-log-index=relay-log-bin.index
relay-log=relay-bin
replicate_wild_ignore_table=mysql.%
replicate_wild_ignore_table=information_schema.%
replicate_wild_ignore_table=performance_schema.%


# 主库配置
server-id=1 # 注意server-id必须唯一, 这里取的是ip地址
log-bin=mysql-bin
binlog-ignore-db=mysql
binlog-ignore-db=information_schema
binlog-ignore-db=performance_schema

```

### 1.3 启动容器

使用docker-compose命令启动容器

```shell

docker-compose up -d

```

## 2. 连接主从节点

首先进入docker的mysql容器

```shell
docker exec -it 容器id /bin/bash
```

进入MySQL服务

```shell
mysql -uroot -p123456
```

进入MySQL服务, 查询server-id是否是宿主机配置的server-id, 检查配置文件生效

### 2.1 配置主库

创建用户并授权

```sql
create user 'repl' identified by `repl`

grant replication slave on *.* to 'repl'@'192.168.100.%'
```

### 2.2 配置从库

```sql

# 连接主库

change master to master_host='host',master_port=3306,master_user='repl',master_password='repl',master_log_file='master-bin.000001',master_log_pos=0;

# 启动slave

start slave;

# 查看连接状态

show slave status \G;
```

### 2.3 查看状态

经过上面的操作, 如下图所示, 即表示连接成功, 否则根据失败的日志, 查找原因

![MySQL_Cluster](/images/MySQL_Cluster.png)


