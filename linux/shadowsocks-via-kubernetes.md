---
title: Shadowsocks via kubernetes
description: 
published: true
date: 2022-06-24T21:30:50.457Z
tags: 
editor: markdown
dateCreated: 2022-06-24T21:30:50.457Z
---

# Shadowsocks via kubernetes
## Install Kubernetes
- You must have Kubernetes installed. 
- You should also have a local configured copy of kubectl.

An example for Microk8s in openSUSE:
```bash
# https://snapcraft.io/install/microk8s/opensuse
# Install snap
sudo zypper addrepo --refresh https://download.opensuse.org/repositories/system:/snappy/openSUSE_Leap_15.2 snappy
sudo zypper --gpg-auto-import-keys refresh
sudo zypper dup --from snappy
sudo zypper install snapd
# Reboot and then do next:
# enable snapd.service
sudo systemctl enable snapd
sudo systemctl start snapd
# enable snapd.apparmor for openSUSE Tumbleweed version
sudo systemctl enable snapd.apparmor
sudo systemctl start snapd.apparmor
```

## 