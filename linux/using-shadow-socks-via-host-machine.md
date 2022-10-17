---
title: Using shadowsocks via host machine
description: 
published: true
date: 2022-10-17T09:53:20.657Z
tags: 
editor: markdown
dateCreated: 2022-10-17T09:25:25.169Z
---

# Using shadow socks via host machine
- Configure your virtual network adpater connection as NAT
- Configure Shadowsocks client to "Allow other Devices to connect" in system tray.
- Check your ip address in your virtual machine, if your virtual machine's ip is 192.168.a.b, then your virtual host ip is 192.168.a.1
- Configure your proxy in vmware as:
ip address  | port | protocol 
----------- | ---- | --------
192.168.a.1 | 1080 | socks5
