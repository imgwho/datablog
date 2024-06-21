---
date: 2024-06-18
title: CentOS 9 安装 MySQL 8
tags:
  - Linux
  - MySQL
description: 摘要：本文主要介绍在CentOS环境下安装MySQL的关键步骤和注意事项等。
---

# CentOS 9 安装MySQL 8

## 通过软件包安装

在这个网站找到最新的仓库<https://dev.mysql.com/downloads/repo/yum/>，根据版本需要选择第一个  Red Hat Enterprise Linux 9 / Oracle Linux 9，点击下载，在跳转的新页面右键点击"No thanks, just start my download."，复制链接，拿到最新的存储库地址。

补充知识：dnf命令是 Fedora 和基于 Fedora 的系统（如 CentOS 和 RHEL 8 及以后版本）使用的软件包管理器。它用于安装、更新和删除软件包。  
RPM 包的 URL是一个网址，安装这个包会在你的系统上配置 MySQL 存储库，从而使你能够使用 dnf 安装和更新 MySQL 相关的软件包。

```
# 安装基础环境依赖
yum install -y gcc gcc-c++ automake pcre pcre-devel zip openssl pcre-devel libtool make kernel-devel

sudo dnf install -y https://dev.mysql.com/get/mysql84-community-release-el9-1.noarch.rpm
sudo dnf install mysql-community-server
```
## 初始化

```
sudo mysqld --initialize --user=mysql --lower-case-table-names=1
```


## 修改配置(注释掉原来的)
```
# 查看root密码
grep "password" /var/log/mysqld.log

vi /etc/my.cnf

[mysqld] 
lower-case-table-names=1
default-time_zone='+8:00'
max_connections=1000
max_connect_errors=1000
# default-authentication-plugin=mysql_native_password mysql 8.4 用下面这行替换这个
mysql_native_password=ON
character-set-server=utf8
sql_mode='STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_ENGINE_SUBSTITUTION'

[client]
default-character-set=utf8
```

## 重启服务
```
systemctl restart mysqld

# 连接MySQL
mysql -u root -p
```

## 错误排查
```
# 服务启动失败，先看配置文件
[root@centos-s-2vcpu-2gb-nyc3-01 ~]# mysqld --validate-config
2024-06-20T00:36:37.507146Z 0 [ERROR] [MY-000067] [Server] unknown variable 'default-authentication-plugin=mysql_native_password'.
2024-06-20T00:36:37.507422Z 0 [ERROR] [MY-010119] [Server] Aborting
```

<Comment />