---
title: Using shadowsocks via host machine
description: 
published: true
date: 2022-10-17T09:25:25.169Z
tags: 
editor: markdown
dateCreated: 2022-10-17T09:25:25.169Z
---

# Using shadow socks via host machine
- 虚拟机网络使用 NAT 桥接
- shadowsocks (Win) 打开 “允许来自外部的连接”
- 在虚拟机和主机中查看 ip，终端命令如ip addr 或 ifconfig 或 ipconfig。
- 虚拟机中 ip 形如 192.168.a.b，那么在主机中应该有名字中包含 vmware 的虚拟网络 ip 为 192.168.a.1。
- 在虚拟机中代理选择 192.168.a.1，端口默认是 1080，协议 socks5。