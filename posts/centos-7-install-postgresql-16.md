---
date: 2024-06-08
title: CentOS 7 安装PostgreSQL 16
tags:
  - Linux
  - PostgreSQL
description: 摘要：本文主要介绍在CentOS环境下安装PostgreSQL的关键步骤和注意事项等。
---

# CentOS 7.9 安装PostgreSQL

先是centos 9 安装的话，步骤如下：
```

# 更新软件包
sudo dnf update -y

# Install the repository RPM:
sudo dnf install https://download.postgresql.org/pub/repos/yum/reporpms/EL-9-x86_64/pgdg-redhat-repo-latest.noarch.rpm -y

sudo dnf -qy module disable postgresql

# Install PostgreSQL:
sudo dnf install postgresql16-server -y

# Optionally initialize the database and enable automatic start:
sudo /usr/pgsql-16/bin/postgresql-16-setup initdb
sudo systemctl enable postgresql-16
sudo systemctl start postgresql-16

# 连接postgre，首先切换用户
[root@centos-s-2vcpu-2gb-nyc3-01 ~]# su - postgres
[postgres@centos-s-2vcpu-2gb-nyc3-01 ~]$ psql
psql (16.3)
Type "help" for help.

postgres=# \du
                             List of roles
 Role name |                         Attributes
-----------+------------------------------------------------------------
 postgres  | Superuser, Create role, Create DB, Replication, Bypass RLS

```



下面是低版本的centos系统，源码安装
## 创建postgres用户来安装

```
useradd -m postgres
passwd postgres
usermod -aG wheel postgres

#切换用户
su - postgres

```
## wget下载源码，yum安装依赖
下载源码 
```
wget https://ftp.postgresql.org/pub/source/v16.3/postgresql-16.3.tar.gz

We trust you have received the usual lecture from the local System
Administrator. It usually boils down to these three things:

    #1) Respect the privacy of others.
    #2) Think before you type.
    #3) With great power comes great responsibility.

[sudo] password for postgres:
--2024-06-08 02:03:05--  https://ftp.postgresql.org/pub/source/v16.3/postgresql-16.3.tar.gz
Resolving ftp.postgresql.org (ftp.postgresql.org)... 217.196.149.55, 147.75.85.69, 72.32.157.246, ...
Connecting to ftp.postgresql.org (ftp.postgresql.org)|217.196.149.55|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 32616059 (31M) [application/octet-stream]
Saving to: ‘postgresql-16.3.tar.gz’

100%[===========================================================================>] 32,616,059  17.4MB/s   in 1.8s

2024-06-08 02:03:07 (17.4 MB/s) - ‘postgresql-16.3.tar.gz’ saved [32616059/32616059]
```

安装依赖
```
yum install -y perl-ExtUtils-Embed readline-devel zlib-devel pam-devel libxml2-devel libxslt-devel openldap-devel python-devel gcc-c++ openssl-devel cmake
```

## 编译和安装
```
# 创建安装目录
mkdir -p /data/server/pgsql

# 创建数据目录
mkdir -p /data/server/pgsql/data

# 解压编译安装
tar -zxvf postgresql-16.0.tar.gz
cd postgresql-16.0
./configure --prefix=/data/server/pgsql --without-icu --with-systemd
# 上面一步报错就安装这个然后重新执行上一步
yum install systemd-devel

make && make install

# 修改目录属主、属组
chown -R postgres:postgres /data/server/pgsql

# 切换到postgres用户初始化数据目录
su - postgres
/data/server/pgsql/bin/initdb -D /data/server/pgsql/data

The files belonging to this database system will be owned by user "postgres".
This user must also own the server process.

The database cluster will be initialized with locale "en_US.UTF-8".
The default database encoding has accordingly been set to "UTF8".
The default text search configuration will be set to "english".

Data page checksums are disabled.

fixing permissions on existing directory /data/server/pgsql/data ... ok
creating subdirectories ... ok
selecting dynamic shared memory implementation ... posix
selecting default max_connections ... 100
selecting default shared_buffers ... 128MB
selecting default time zone ... UTC
creating configuration files ... ok
running bootstrap script ... ok
performing post-bootstrap initialization ... ok
syncing data to disk ... ok

initdb: warning: enabling "trust" authentication for local connections
initdb: hint: You can change this by editing pg_hba.conf or using the option -A, or --auth-local and --auth-host, the next time you run initdb.

Success. You can now start the database server using:

    /data/server/pgsql/bin/pg_ctl -D /data/server/pgsql/data -l logfile start
```
## 配置PostgreSQL 服务，可以开机自启
```
vim /etc/systemd/system/postgresql.service

# 配置以下内容
[Unit]
Description=PostgreSQL 16.0 database server
Documentation=man:postgres(1)
After=network-online.target
Wants=network-online.target

[Service]

Type=forking
User=postgres
Group=postgres
OOMScoreAdjust=-1000
ExecStart=/data/server/pgsql/bin/pg_ctl start -D /data/server/pgsql/data 
ExecStop=/data/server/pgsql/bin/pg_ctl stop -D /data/server/pgsql/data
ExecReload=/data/server/pgsql/bin/pg_ctl reload -D /data/server/pgsql/data 
TimeoutSec=0

[Install]
WantedBy=multi-user.target

```

## 启动PostgreSQL 服务

```
# 没有systemctl的话就安装一下，yum install firewalld -y

# 打开防火墙
systemctl start firewalld
systemctl enable firewalld


systemctl daemon-reload
systemctl enable postgresql
systemctl start postgresql

# postree查看进程，没有这个命令用yum install psmisc -y
pstree
systemd─┬─2*[agetty]
        ├─anacron
        ├─auditd───{auditd}
        ├─chronyd
        ├─crond
        ├─dbus-daemon───{dbus-daemon}
        ├─droplet-agent───7*[{droplet-agent}]
        ├─gssproxy───5*[{gssproxy}]
        ├─irqbalance
        ├─master─┬─pickup
        │        └─qmgr
        ├─polkitd───6*[{polkitd}]
        ├─postgres───5*[postgres]
        ├─rpcbind
        ├─rsyslogd───2*[{rsyslogd}]
        ├─sshd─┬─sshd───bash───su───bash───su───bash───su───bash───su───bash───su───bash───su───bash───su───bash───su───bash───su───bash───su───+
        │      └─sshd───sftp-server
        ├─systemd-journal
        ├─systemd-logind
        ├─systemd-udevd
        └─tuned───4*[{tuned}]
```
## 连接数据库
```
# 切换到 postgres用户
su - postgres
# 连接postgre
/data/server/pgsql/bin/psql 

# 创建用户及密码
postgres=# create user gadmin password '123456';

# 创建指定拥有者gadmin的数据库tt
postgres=# create database tt owner gadmin;
```

## 修改配置，允许远程连接
```
# 找到配置文件
sudo find / -name postgresql.conf
[sudo] password for postgres:
/data/server/pgsql/data/postgresql.conf

# 对于这个配置
vi /data/server/pgsql/data/postgresql.conf

# vi到文件开头:gg，到文件末尾:$，这里直接到末尾，然后添加以下配置

# 修改监听地址，否则无法远程连接
listen_addresses = '*'
port = 5432
# 修改最大连接数
max_connections = 1024
# 设置socket目录
unix_socket_directories = '/data/server/pgsql'
# 开启日志获取
logging_collector = on
# 设置日志目录
log_directory = 'pg_log'
# 设置日志文件名称格式
log_filename = 'postgresql-%Y-%m-%d.log'
# 开启日志轮转
log_truncate_on_rotation = on 


vi /data/server/pgsql/data/pg_hba.conf

# 在这个位置
# IPv4 local connections:
host    all             all             127.0.0.1/32            trust
# 添加一行，允许远程连接
# tt为数据库名，gadmin为用户名，后面为允许连接的地址范围，md5使用密码验证
host    all             all             0.0.0.1/0            md5

# 重启服务后就能使用gadmin账户远程连接tt数据库了
systemctl restart postgresql

```


<Comment />