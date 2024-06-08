---
date: 2024-06-08
title: CentOS 7 安装 MySQL 8
tags:
  - Linux
  - MySQL
description: 摘要：本文主要介绍在CentOS环境下安装MySQL的关键步骤和注意事项等。
---

# CentOS 7.9 安装PostgreSQL

## 通过源安装

```
sudo rpm -Uvh https://dev.mysql.com/get/mysql80-community-release-el7-3.noarch.rpm
sudo yum install --nogpgcheck mysql-community-server
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
default-authentication-plugin=mysql_native_password
character-set-server=utf8
sql_mode='STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_ENGINE_SUBSTITUTION'

[client]
default-character-set=utf8
```

## 重启服务
```
systemctl restart mysqld
```

<Comment />