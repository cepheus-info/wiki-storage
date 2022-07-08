---
title: Getting started of kubernetes
description: 
published: true
date: 2022-07-08T09:56:49.537Z
tags: 
editor: markdown
dateCreated: 2022-06-27T08:19:40.530Z
---

# Getting started of kubernetes
## Install Kubernetes
- You must have Kubernetes installed. 
- You should also have a local configured copy of kubectl.

An example for Microk8s in openSUSE:
```bash
# https://snapcraft.io/install/microk8s/opensuse
# install snap
sudo zypper addrepo --refresh https://download.opensuse.org/repositories/system:/snappy/openSUSE_Tumbleweed snappy
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
# install microk8s:
sudo snap install microk8s --classic
```
Note that there might be an error occurring when start snapd service. You should first install any snaps so that the snapd would keep running in background.

Install kubernetes-client
```bash
sudo zypper install kubernetes1.24-client
```

## Install Helm
You must have Kubernetes installed. For the latest release of Helm, we recommend the latest stable release of Kubernetes, which in most cases is the second-latest minor release.
You should also have a local configured copy of kubectl.

Install helm via snap
```bash
sudo snap install helm --classic
```

Install helm via package manager:
```bash
sudo zypper install helm
```

Initialize a Helm Chart Repository
```bash
helm repo add bitnami https://charts.bitnami.com/bitnami
```
