---
date: 2025-12-16
title: 阿里云 Debian 全能服务器搭建指南：从 Sing-box 到 Claude Code
tags:
  - Linux/Debian
  - Docker
  - Sing-box
  - n8n
  - AI/DevOps
description: 摘要：一份阿里云 Debian 服务器的“黄金路径”配置手册。本文详细记录了如何解决国内服务器网络环境问题，通过 Sing-box 实现分流，配置 Docker 镜像加速，并部署 n8n 自动化与 Claude Code AI 编程环境。
---


这份文档详细记录了在 **阿里云 Debian (Trixie/Testing)** 服务器上搭建 **全能自动化与 AI 开发环境** 的完整过程。

文档基于您之前的实际操作整理，去除了试错步骤，只保留**最终、正确、可复现**的“黄金路径”。

-----

# 阿里云 Debian 服务器全栈配置手册

**目标**：构建一台集成了全局网络加速、Docker 容器化、n8n 自动化工作流以及 Claude Code AI 编程能力的生产力服务器。
**环境**：阿里云 ECS / Debian GNU/Linux (Trixie/Sid)
**前置要求**：以 `root` 用户登录。

-----

## 第一部分：系统安全加固 (SSH 密钥登录)

为了防止暴力破解，禁用密码登录，仅允许密钥认证。

### 1.1 客户端生成密钥 (在您本地电脑)

```bash
# 生成 Ed25519 密钥对
ssh-keygen -t ed25519 -C "aliyun_server"
# 获取公钥内容 (复制输出的 ssh-ed25519 开头字符串)
cat ~/.ssh/id_ed25519.pub
```

### 1.2 服务器端配置 (在阿里云)

```bash
# 创建目录并写入公钥
mkdir -p ~/.ssh
chmod 700 ~/.ssh
nano ~/.ssh/authorized_keys
# (在此粘贴您的公钥，保存退出)
chmod 600 ~/.ssh/authorized_keys

# 修改 SSH 配置文件，禁用密码
nano /etc/ssh/sshd_config
```

**修改以下项：**

```text
PubkeyAuthentication yes
PasswordAuthentication no
```

**重启 SSH 服务：**

```bash
systemctl restart ssh
```

-----

## 第二部分：网络核心基础设施 (Sing-box)

这是服务器的流量网关，负责分流国内外流量。

### 2.1 安装 Sing-box

```bash
# 下载安装包 (根据架构选择，通常是 amd64)
VERSION="1.9.0" # 举例，请使用最新版
wget https://github.com/SagerNet/sing-box/releases/download/v${VERSION}/sing-box_${VERSION}_linux_amd64.deb
dpkg -i sing-box_${VERSION}_linux_amd64.deb

# 启用服务
systemctl enable sing-box
```

### 2.2 下载路由规则文件

```bash
mkdir -p /etc/sing-box
# 下载 geosite 和 geoip 的二进制规则文件
wget -O /etc/sing-box/geosite-cn.srs https://github.com/SagerNet/sing-geosite/releases/latest/download/geosite-cn.srs
wget -O /etc/sing-box/geoip-cn.srs https://github.com/SagerNet/sing-geoip/releases/latest/download/geoip-cn.srs
```

### 2.3 写入配置文件 (VLESS + Vision + 自动分流)

**执行以下命令覆盖配置：**

```bash
cat > /etc/sing-box/config.json <<EOF
{
  "log": { "level": "info", "timestamp": true },
  "dns": {
    "servers": [
      { "tag": "google", "address": "8.8.8.8", "detour": "proxy" },
      { "tag": "local", "address": "223.5.5.5", "detour": "direct" }
    ],
    "rules": [
      { "outbound": "any", "server": "local" },
      { "rule_set": "geosite-cn", "server": "local" },
      { "domain_suffix": [".anthropic.com", ".claude.ai", ".openai.com"], "server": "google" }
    ]
  },
  "inbounds": [
    { "type": "socks", "tag": "socks-in", "listen": "127.0.0.1", "listen_port": 1080 },
    { "type": "http", "tag": "http-in", "listen": "127.0.0.1", "listen_port": 8080 }
  ],
  "outbounds": [
    {
      "type": "vless",
      "tag": "proxy",
      "server": "xxx.cc",
      "server_port": 444,
      "uuid": "xxxx",
      "flow": "xtls-rprx-vision",
      "tls": {
        "enabled": true,
        "server_name": "xxxx",
        "utls": { "enabled": true, "fingerprint": "firefox" },
        "alpn": ["h2", "http/1.1"]
      }
    },
    { "type": "direct", "tag": "direct" },
    { "type": "dns", "tag": "dns-out" }
  ],
  "route": {
    "rule_set": [
      { "tag": "geosite-cn", "type": "local", "format": "binary", "path": "/etc/sing-box/geosite-cn.srs" },
      { "tag": "geoip-cn", "type": "local", "format": "binary", "path": "/etc/sing-box/geoip-cn.srs" }
    ],
    "rules": [
      { "protocol": "dns", "outbound": "dns-out" },
      { "domain_suffix": ["api.anthropic.com", "claude.ai", "openai.com"], "outbound": "proxy" },
      { "rule_set": ["geosite-cn", "geoip-cn"], "outbound": "direct" },
      { "ip_is_private": true, "outbound": "direct" }
    ],
    "auto_detect_interface": true,
    "final": "proxy"
  }
}
EOF
```

### 2.4 启动与验证

```bash
systemctl restart sing-box
systemctl status sing-box
# 验证：curl -x http://127.0.0.1:8080 -I https://www.google.com 应返回 200 OK
```

-----

## 第三部分：Shell 环境配置 (快捷指令)

配置 `proxy` 和 `unproxy` 别名，方便终端操作。

### 3.1 修改 .bashrc

```bash
nano ~/.bashrc
```

**在文件末尾添加：**

```bash
# 网络代理快捷键
alias proxy='export http_proxy=http://127.0.0.1:8080 https_proxy=http://127.0.0.1:8080 all_proxy=socks5://127.0.0.1:1080'
alias unproxy='unset http_proxy https_proxy all_proxy'

# Claude Code 快捷启动 (预设中转配置)
# 请替换您的 Key 和 URL
export NODE_TLS_REJECT_UNAUTHORIZED=0
export ANTHROPIC_API_KEY="sk-xxxxxxxx" 
export ANTHROPIC_BASE_URL="https://您的中转域名/v1"
```

**生效：**

```bash
source ~/.bashrc
```

-----

## 第四部分：Docker 容器平台

解决阿里云 Debian 安装 Docker 慢及拉取镜像被墙的问题。

### 4.1 安装 Docker 引擎

采用“分离法”安装（代理下载脚本，直连安装软件）。

```bash
# 1. 开启代理下载脚本
proxy
curl -fsSL https://get.docker.com -o install.sh

# 2. 关闭代理 (重要：否则无法连接阿里云内网 apt 源)
unproxy

# 3. 指定阿里云镜像源安装 (解决 Trixie 版本兼容问题)
DOWNLOAD_URL="https://mirrors.aliyun.com/docker-ce" LS_RELEASE="bookworm" sh install.sh --mirror Aliyun

# 4. 启动 Docker
systemctl enable --now docker
```

### 4.2 配置 Docker 守护进程代理 (关键)

让 Docker 后台服务通过 Sing-box 拉取国外镜像。

```bash
mkdir -p /etc/systemd/system/docker.service.d
cat > /etc/systemd/system/docker.service.d/http-proxy.conf <<EOF
[Service]
Environment="HTTP_PROXY=http://127.0.0.1:8080"
Environment="HTTPS_PROXY=http://127.0.0.1:8080"
Environment="NO_PROXY=localhost,127.0.0.1,mirrors.aliyun.com,mirrors.cloud.aliyuncs.com"
EOF

# 重载配置并重启
systemctl daemon-reload
systemctl restart docker
```

-----

## 第五部分：自动化工作流 (n8n)

部署 n8n 并打通网络。

### 5.1 准备数据目录与权限

解决 n8n 容器启动后因权限不足无限重启的问题。

```bash
mkdir -p ~/n8n_data
# 1000 是 n8n 容器内部用户的 UID
chown -R 1000:1000 ~/n8n_data
```

### 5.2 启动容器 (Host 模式)

使用 `host` 网络模式，使容器能直接访问宿主机的 8080 代理端口。

```bash
docker run -d \
  --name n8n \
  --network host \
  --restart always \
  -e GENERIC_TIMEZONE=Asia/Shanghai \
  -e N8N_SECURE_COOKIE=false \
  -e HTTP_PROXY="http://127.0.0.1:8080" \
  -e HTTPS_PROXY="http://127.0.0.1:8080" \
  -e ALL_PROXY="socks5://127.0.0.1:1080" \
  -v ~/n8n_data:/home/node/.n8n \
  n8nio/n8n:latest
```

### 5.3 阿里云防火墙设置

**必须操作**：登录阿里云控制台 -\> 安全组 -\> 入方向 -\> 放行 TCP 端口 `5678`。

-----

## 第六部分：AI 编程助手 (Claude Code)

### 6.1 安装运行时 (Bun / Node)

```bash
# 开启代理
proxy
# 安装 Bun (自带 Node 兼容)
curl -fsSL https://bun.sh/install | bash
source ~/.bashrc
```

### 6.2 安装 cc-cli

```bash
# 通过 npm/bun 全局安装
npm install -g @anthropic-ai/claude-code
# 或者
bun add -g @anthropic-ai/claude-code
```

### 6.3 验证

直接输入 `cc` (因为在第三部分已经在 .bashrc 里配置了 `ANTHROPIC_API_KEY` 等变量，此时应直接进入交互界面)。

-----

## 总结：如何使用这台服务器

1.  **日常维护**： SSH 登录。
2.  **安装软件/Git Clone**：先输 `proxy`，再执行命令。
3.  **系统更新 (apt update)**：先输 `unproxy`，再执行命令。
4.  **n8n 访问**：浏览器打开 `http://<公网IP>:5678`。
5.  **AI 写代码**：终端输入 `cc`，用中文描述需求。
6.  **Docker 部署新服务**：直接 `docker run`，无需额外配置，镜像拉取会自动走代理。



<Comment />
