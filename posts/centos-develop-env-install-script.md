---
date: 2024-06-18
title: CentOS 常用开发环境搭建
tags:
  - Linux
  - Shell
description: 摘要：本文主要介绍在CentOS环境下安装软件之前，搭建一些必要的库的关键步骤和注意事项等。
---

# CentOS 常用环境搭建

## 环境搭建脚本
新建一个sh文件，赋予权限，然后执行安装  
install.sh
```
#!/bin/sh
yum -y update
 
yum install epel-release -y
 
#安装大而全的基础开发调试工具
#yum groupinstall "Development Tools"
#用于上传下载资源
yum install -y wget man libtool-ltdl libtool-ltdl-devel jq
#编译安装git
yum install -y curl curl-devel expat-devel gettext-devel  openssl-devel zlib-devel autoconf perl-ExtUtils-CBuilder perl-ExtUtils-MakeMaker
#编译安装nginx需要
yum install -y gcc gcc-c++ automake pcre pcre-devel zip openssl pcre-devel libtool make kernel-devel
#编译安装php需要
yum install -y libxml2 libxml2-devel libjpeg libjpeg-devel libpng libpng-devel freetype freetype-devel libxslt libxslt-devel bzip2 bzip2-devel
#安装常用工具
#apache 压力测试工具
yum install -y httpd-tools net-tools.x86_64
#php8
yum install -y sqlite-devel libcurl-devel  libzip-devel  krb5-devel libicu-devel 
```


<Comment />