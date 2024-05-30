---
date: 2022-06-30
title: tableau server 安装
# category: 主题
tags:
- tableau server
description: tableau server 安装
---
# Tableau server 安装

# 安装步骤

## 一、安装前准备

1. 硬件需求:根据 Tableau Server 的部署规模，准备合适配置的服务器，至少应满足最低硬件要求。
2. 操作系统要求:确保服务器操作系统满足要求，目前支持的操作系统包括:

- RHEL 7.3 及更高版本
- CentOS 7.3 及更高版本
- Oracle Linux 7.3 及更高版本
- Ubuntu 16.04 LTS 及更高版本

1. 网络和目录配置:

- 确保主机名(hostname)符合要求，将其添加到 hosts 文件中

```markdown
hostnamectl set-hostname <新主机名>
```

- 创建数据目录，确保运行 Tableau Server 进程的用户对相应目录有读写权限

## 二、安装 Tableau Server 软件包

<!-- ![](static/JAX2bDREtoTVhQxbSCxcDAIUnKf.png) -->

1. 下载适用于 Linux 的 Tableau Server 安装包，解压缩到安装目录，例如:

```
tar -xvf tableau-server-<version>.x86_64.rpm
```

1. 运行安装脚本:

```
sudo ./initialize-tsm --accepteula
```

## 三、初始化和激活 Tableau Server

1. 初始化 Tableau 服务管理器(TSM):

```
sudo /opt/tableau/tableau_server/packages/scripts.<version>/initialize-tsm --accepteula
```

1. 使用 TSM Web 界面或命令激活 Tableau Server:

- Web 界面:访问 https://<hostname>:8850
- 命令行:

```
tsm licenses activate -k <product-key>
```

1. 注册 Tableau Server:

```
tsm register --file /path/to/registration_file.json
```

## 四、配置初始设置

1. 配置身份存储、网关端口等必要设置，可以使用 TSM Web 界面或命令行:

```
tsm configuration set -k wgserver.authenticate -v local
tsm configuration set -k gateway.public.port -v 80
```

1. 创建初始管理员账号:

```
tabcmd initialuser --server http://<hostname> --username "<admin-username>" --password "<password>"
```

## 五、应用配置并启动服务器

1. 应用 pending 状态的配置并重启服务器:

```
tsm pending-changes apply
```

1. 初始化并启动 Tableau Server:

```
tsm initialize --start-server --request-timeout 1800
```

## 六、验证安装

1. 访问 Tableau Server 的 Web 界面，使用初始管理员账号登录，导入数据源、发布工作簿等，检查各项功能是否正常。
2. 检查各进程和服务的状态:

```
tsm status -v
```

## 七、其他配置

1. 生成引导文件以进行多节点部署:

```
tsm topology nodes get-bootstrap-file --file /tmp/bootstrap.json
```

1. 根据需要配置身份认证、数据源、网络代理、SMTP 等各项设置。
2. 执行安全加固，如:禁用未使用的服务、配置 SSL/TLS、设置会话生命周期等。
3. 定期检查 Tableau Server 的运行状态，确保及时更新到最新版本并打上必要的安全补丁。

# 考试指南 部分

请注意：这不是本考试内容的全面清单。

[https://www.tableau.com/zh-cn/learn/certification/tableau-server-certified-associate-exam-guide](https://www.tableau.com/zh-cn/learn/certification/tableau-server-certified-associate-exam-guide)

### 领域 1：连接到数据并准备数据



**5.4 了解向后兼容性**


<Comment />


