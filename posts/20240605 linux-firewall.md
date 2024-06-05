---
date: 2024-06-05
title: Linux 防火墙开放端口
tags:
  - linux
description: 摘要：本文主要介绍linux上面如何开放需要的服务的端口，避免服务访问失败。
---

# Linux 防火墙开放端口
# 起因  

服务器之前运行的一个服务，今天突然无法使用，我用tcp.ping.pe这个网站查看，发现端口ping不通
![screenshot](https://pub-833348ee5761457dbfac749bcd651384.r2.dev/datablog%2Fcdc755b69f023db8f21e84494e6844abc0c0fdcd07bebfaa17231de8b29e3a84.webp)

# 排查
先把防火墙关了，看到服务的状态正常
```
systemctl stop firewalld; systemctl disable firewalld; ufw disable
```
# 解决
所以目前只需要开放这个服务的端口即可。同时使用 firewalld 和 ufw 可能会导致冲突和意外行为，通常建议只使用其中一个防火墙管理工具，这里保留firewalld。

```
root@NewVPS-WIKIHOST-190529WDGFQW:~# sudo firewall-cmd --list-ports

root@NewVPS-WIKIHOST-190529WDGFQW:~# sudo firewall-cmd --list-all
public
  target: default
  icmp-block-inversion: no
  interfaces:
  sources:
  services: dhcpv6-client ssh
  ports:
  protocols:
  masquerade: no
  forward-ports:
  source-ports:
  icmp-blocks:
  rich rules:
```
运行 sudo firewall-cmd --list-ports 命令时，没有显示任何开放的端口。这意味着在 firewalld 的当前配置中，没有明确指定要开放的端口。  

运行 sudo firewall-cmd --list-all 命令时，显示了 firewalld 的默认区域 public 的详细配置:
```
target: default:表示默认的目标策略是 default，即允许所有传出流量并拒绝所有传入流量，除非明确允许。
icmp-block-inversion: no:表示不反转 ICMP 阻止规则。
interfaces:没有列出任何接口，表示该区域适用于所有未明确分配给其他区域的网络接口。
sources:没有列出任何源地址，表示该区域适用于所有未明确分配给其他区域的流量源。
services: dhcpv6-client ssh:表示允许 DHCPv6 客户端和 SSH 服务的流量。
ports:没有列出任何端口，表示没有明确开放任何端口。
protocols:没有列出任何协议，表示没有明确允许任何特定协议。
masquerade: no:表示不启用 IP 伪装(NAT)。
forward-ports:没有列出任何端口转发规则。
source-ports:没有列出任何源端口规则。
icmp-blocks:没有列出任何要阻止的 ICMP 类型。
rich rules:没有列出任何富规则(高级过滤规则)。
根据这些信息，可以得出结论:在当前的 firewalld 配置中，  
除了允许 DHCPv6 客户端和 SSH 服务的流量外，没有明确开放任何端口。
```

那么现在的操作就很简单了，在防火墙里开放需要的端口即可，命令如下，--permanent 选项确保在防火墙重启后规则仍然生效。--reload 命令重新加载防火墙规则，使更改立即生效。
```
root@NewVPS-WIKIHOST-190529WDGFQW:~# sudo firewall-cmd --zone=public --add-port=10520/tcp --permanent
success
root@NewVPS-WIKIHOST-190529WDGFQW:~# sudo firewall-cmd --zone=public --add-port=10520/udp --permanent
success
root@NewVPS-WIKIHOST-190529WDGFQW:~# sudo firewall-cmd --reload
success
root@NewVPS-WIKIHOST-190529WDGFQW:~# sudo firewall-cmd --zone=public --list-ports
10520/tcp 10520/udp
```

# 总结
以下是一些典型的例子:

1. 开放或关闭特定端口:
```
#开放端口:
sudo firewall-cmd --zone=public --add-port=80/tcp --permanent
sudo firewall-cmd --reload
#关闭端口:
basic
sudo firewall-cmd --zone=public --remove-port=80/tcp --permanent
sudo firewall-cmd --reload
```
2. 允许或阻止特定服务:
```
#允许服务:
sudo firewall-cmd --zone=public --add-service=http --permanent
sudo firewall-cmd --reload
#阻止服务:
basic
sudo firewall-cmd --zone=public --remove-service=ssh --permanent
sudo firewall-cmd --reload
```
3. 配置特定网络接口的区域:
```
sudo firewall-cmd --zone=trusted --change-interface=eth0 --permanent
sudo firewall-cmd --reload
```
4. 允许或阻止特定 IP 地址:
```
#允许 IP:
sudo firewall-cmd --zone=public --add-source=192.168.1.100 --permanent
sudo firewall-cmd --reload
#阻止 IP:
basic
sudo firewall-cmd --zone=public --remove-source=192.168.1.100 --permanent
sudo firewall-cmd --reload
```
5. 配置端口转发:
```
sudo firewall-cmd --zone=public --add-forward-port=port=80:proto=tcp:toport=8080 --permanent
sudo firewall-cmd --reload
```
6. 检查防火墙状态和规则:
```
#检查 firewalld 状态:
sudo systemctl status firewalld
#列出所有规则:
sudo firewall-cmd --list-all
#列出特定区域的规则:
sudo firewall-cmd --zone=public --list-all
```
7. 管理预定义服务:
```
#列出所有预定义服务:
sudo firewall-cmd --get-services
#检查服务的默认配置:
sudo firewall-cmd --info-service=http
```

<Comment />