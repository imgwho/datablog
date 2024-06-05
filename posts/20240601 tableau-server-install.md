---
date: 2024-06-01
title: Tableau Server 安装实操(2022.1为例)
tags:
  - tableau server
  - linux
description: 摘要：本文主要介绍Tableau server 安装的关键步骤和注意事项等。
---
# Tableau Server 安装实操(2022.1为例)

# 安装步骤
此教程主要介绍安装和配置TableauServer的步骤，且只针对单节点。 运行安装之后，必须通过激活许可证、注册TableauServer和配置包括身份验证 在内的各种设置来继续进行设置，主要有以下步骤：
1. 安装和初始化TSM
2. 激活并注册TableauServer 
3. 添加管理员帐户 
4. 验证安装
## 一、安装前准备

1. 硬件需求: 版本 2022.1 及更低版本至少8核16g，版本 2022.3 及更高版本内存至少64g。
2. 操作系统要求: 确保服务器操作系统满足要求，目前支持的操作系统包括:
- RHEL 7.3 及更高版本
- CentOS 7.3 及更高版本
- Oracle Linux 7.3 及更高版本
- Ubuntu 16.04 LTS 及更高版本

- 准备专用用户来安装Tableau Server
```
useradd -m tabadmin
passwd tabadmin
usermod -aG wheel tabadmin

su - tabadmin
```

## 二、安装 Tableau Server 软件包

点进目前官网发现有以下大版本：2023.3 2023.1 2022.3 2022.1 2021.4 2021.3 2021.2  [https://www.tableau.com/support/releases/server](https://www.tableau.com/support/releases/server)  
点击需要的版本找到下载地址（2022.1.23为例）
[https://www.tableau.com/support/releases/server/2022.1.23#esdalt](https://www.tableau.com/support/releases/server/2022.1.23#esdalt)  
这里可以看到2022.1.23的linux安装包下载地址是
[https://downloads.tableau.com/esdalt/2022.1.23/tableau-server-2022-1-23.x86_64.rpm](https://downloads.tableau.com/esdalt/2022.1.23/tableau-server-2022-1-23.x86_64.rpm)，一般用wget下载  

![图 0](https://pub-833348ee5761457dbfac749bcd651384.r2.dev/datablog%2F8f87792d7c92309cc00bfa0555933b3debda5f65facc273421f65c385ab18c52.webp)  


1. 下载适用于 Linux 的 Tableau Server 安装包
```
wget https://downloads.tableau.com/esdalt/2022.1.23/tableau-server-2022-1-23.x86_64.rpm
```

1. 运行安装脚本
```
sudo yum install tableau-server-2022-1-23.x86_64.rpm -y


[2024-06-03 09:59:42]  If this is a single node or initial node installation, run:
[2024-06-03 09:59:42]      sudo /opt/tableau/tableau_server/packages/scripts.20221.24.0308.1659/initialize-tsm --accepteula
[2024-06-03 09:59:42]  to continue setting up Tableau Server. If this installation is part of a multi-node configuration,
[2024-06-03 09:59:42]  see the online documentation for installing Tableau Server on additional nodes.
[2024-06-03 09:59:42]  
  Verifying  : tableau-server-20221.24.0308.1659-20221-24.0308.1659.x86_64                                    1/1 
[2024-06-03 09:59:43]  Installed:
[2024-06-03 09:59:43]    tableau-server-20221.24.0308.1659.x86_64 0:20221-24.0308.1659                                                  
[2024-06-03 09:59:43]  Complete!
```

## 三、初始化TSM

1. 初始化TSM，关闭ATR激活选项
```
sudo /opt/tableau/tableau_server/packages/scripts.<version>/initialize-tsm --accepteula --no-activation-service

[2024-06-03 10:01:01]  Creating 'tsmadmin' group for TSM admin authorization
[2024-06-03 10:01:01]  Creating 'tableau' unprivileged user account
[2024-06-03 10:01:01]  Creating environment file...
[2024-06-03 10:01:01]  Creating directories and setting permissions...
[2024-06-03 10:01:01]  Using '/var/opt/tableau/tableau_server' as the data directory.
[2024-06-03 10:01:01]  Adding user 'tabadmin' to group 'tableau'...
[2024-06-03 10:01:01]  Adding user 'tabadmin' to group 'tsmadmin'...
[2024-06-03 10:01:01]  Added. Note: These group membership changes do not take effect in shells already open. For these to take effect, log out of the shell and log back in.
[2024-06-03 10:01:01]  Tableau Server runs best with at least 50 GB of free disk space,
[2024-06-03 10:01:01]  but found only 35 GB for the data directory '/var/opt/tableau/tableau_server'. Continuing.
[2024-06-03 10:01:01]  Starting TSM services...
[2024-06-03 10:01:08]  Updating repository version in Tableau Server Coordination Service.
[2024-06-03 10:03:27]  TSM services started successfully
[2024-06-03 10:03:30]  Use the 'tsm' command to continue setting up Tableau Server.
[2024-06-03 10:03:30]  >> Tableau binary directory will be added to PATH for new shells. To get the
[2024-06-03 10:03:30]  >> updated path, either start a new session, or for bash users run:
[2024-06-03 10:03:30]  >> source /etc/profile.d/tableau_server.sh
[2024-06-03 10:03:30]  The TSM administrative web interface (and REST API) is now available at
[2024-06-03 10:03:30]  https://iZbp1587od7awm8s4ol6j2Z:8850
[2024-06-03 10:03:30]  You can continue the configuration and initialization of Tableau server using either the TSM command line interface,
[2024-06-03 10:03:30]  or the web interface.
[2024-06-03 10:03:30]  You will be prompted to authenticate, or can log in using the username 'tabadmin', with the same password you used to log into this session. You could also use any username, with its password, from the administrative group in the domain.
[2024-06-03 10:03:30]  Done.
```

note：可以用以下命令，查看日志判断初始化进度
```
sudo tail -f /var/opt/tableau/tableau_server/data/tabsvc/logs/tabadmincontroller/tabadmincontroller_node1-0.log

[2024-06-03 12:39:22]  2024-06-03 12:39:52.013 +0800  pool-21-thread-1 : INFO  com.tableausoftware.tabadmin.webapp.impl.status.ServiceStatusWatcher - 34 services have reached their expected state
[2024-06-03 12:39:52]  2024-06-03 12:39:52.013 +0800  pool-21-thread-1 : INFO  com.tableausoftware.tabadmin.webapp.impl.status.ServiceStatusWatcher - node1 has 6 services that aren't running: vizportal_0.20221.24.0308.1659 (deployment state: ENABLED, process status: DOWN), backgrounder_0.20221.24.0308.1659 (deployment state: ENABLED, process status: DOWN), vizqlserver_0.20221.24.0308.1659 (deployment state: ENABLED, process status: DOWN), flowprocessor_0.20221.24.0308.1659 (deployment state: ENABLED, process status: STATUS_UNAVAILABLE), minerva_0.20221.24.0308.1659 (deployment state: ENABLED, process status: DOWN), dataserver_0.20221.24.0308.1659 (deployment state: ENABLED, process status: DOWN)
```

## 四、激活和注册Tableau Server

1. 初始化成功后，推荐使用 TSM Web 界面，或用命令行激活和注册 Tableau Server:
- TSM Web 界面激活和注册: 访问 <https://hostname:8850>，在UI页面上进行激活、注册、设置、初始化
- 命令行激活和注册
```
tsm licenses activate -k <product-key>
tsm licenses activate-t  #激活试用版

tsm register--template > /path/to/<registration_file>.json
tsm register --file /path/to/registration_file.json

registration_file.json
{
 "first_name" : "Andrew",
 "last_name" : "Smith",
 "phone" : "311-555-2368",
 "email" : "andrew.smith@mycompany.com",
 "company" : "My Company",
 "industry" : "Finance",
 "company_employees" : "500",
 "department" : "Engineering",
 "title" : "Senior Manager",
 "city" : "Kirkland",
 "state" : "WA",
 "zip" : "98034",
 "country" : "United States",
 "opt_in" : "true",
 "eula" : "true
 }

tsm register--file /usr/share/tableau-reg-file.json
```

2. 配置身份存储、网关端口等必要设置，可以使用 TSM Web 界面或命令行，对应（可选）
```
tsm settings import-f path-to-file.json

path-to-file.json:
{
 "configEntities": {
 "gatewaySettings": {
 "_type": "gatewaySettingsType",
 "port": 80,
 "publicHost": "localhost",
 "publicPort": 80
 },
 "identityStore": {
 "_type": "identityStoreType",
 "type": "local",
 "domain": "example.lan",
 "nickname": "EXAMPLE"
 }
 },
 "configKeys": {
 "gateway.timeout": "900"
}
```
对应TSM Web界面的设置页面，身份存储和网关端口  
![图 1](https://pub-833348ee5761457dbfac749bcd651384.r2.dev/datablog%2F0481a0852d7fb1a60c4a3945fc34ba759ebd95f31960f1ffb5e8b5de4a696752.webp)  


1. 创建初始管理员账号
```
tabcmd initialuser --server http://<hostname> --username "<admin-username>" --password "<password>"

tabcmd initialuser --username brucegavin --server http://localhost #这种方式避免密码暴露
```

## 五、验证安装

1. 访问 Tableau Server 的 Web 界面，使用初始管理员账号登录，导入数据源、发布工作簿等，检查各项功能是否正常。
2. 或者用命令行检查各进程和服务的状态:
```
tsm status -v
```

## 六、其他

如果用root安装了Tableau server，在后面的初始化会出现报错
```
User 'root' has been selected as the user to add to the TSM authorized group, but
TSM does not allow root as a TSM-authorized user. You must either re-run this
script using 'sudo' while logged in as a normal user instead of root (most common
case), rerun this script with the '-a username' option to select a user other than
root to add to the group, or the '-g' flag to disable group addition completely
and add appropriate users to the group yourself. Canceling.
```

可以用下面的命令完全卸载Tableau Server
```
sudo /opt/tableau/tableau_server/packages/scripts.<version_code>/tableau-server-obliterate -a -y -y -y -l
```

升级的话需要先停止tsm，再运行升级脚本，试用版本不能用这个脚本直接升级
```
tsm stop

sudo /opt/tableau/tableau_server/packages/scripts.<version_code>/upgrade-tsm --accepteula

tsm start
```

备份和还原数据
```
(/var/opt/tableau/tableau_server/data/tabsvc/files/backups/ts_backup-2024-06-03.tsbak, #数据的备份文件路径
/home/tabadmin/tss_backup.json) #设置的备份文件路径

tsm maintenance backup -f ts_backup -d -po 
tsm maintenance restore --file ts_backup-2024-06-03.tsbak

tsm settings import -f <filename>.json
tsm pending-changes apply
tsm restart
```


<Comment />
