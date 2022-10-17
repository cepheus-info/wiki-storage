---
title: Getting started of kubernetes
description: 
published: true
date: 2022-10-17T09:32:09.460Z
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
# check microk8s status
microk8s status
```

### Troubleshooting of microk8s

Note that there might be an error occurring when start snapd service. You should first install any snaps so that the snapd would keep running in background.

- If microk8s command-not-found, please check if /snap/bin is in your PATH, otherwise run
  ```bash
  # microk8s command-not-found
  export PATH="$PATH:/snap/bin"
  ```
- If microk8s is not running
  ```bash
  localhost:/ # microk8s inspect 
  Inspecting system
  Inspecting Certificates
  /snap/microk8s/4055/inspect.sh: line 386: openssl: command not found
  WARNING:  This deployments certificates will expire in $days days. 
  Either redeploy MicroK8s or attempt a refresh with "microk8s refresh-certs"
  Inspecting services
  Inspecting AppArmor configuration
  Gathering system information
  Inspecting kubernetes cluster
    Inspect kubernetes cluster
  Inspecting dqlite
    Inspect dqlite

  WARNING:  Docker is installed. 
  Add the following lines to /etc/docker/daemon.json: 
  {
      "insecure-registries" : ["localhost:32000"] 
  }
  and then restart docker with: sudo systemctl restart docker
  WARNING:  The memory cgroup is not enabled. 
  The cluster may not be functioning properly. Please ensure cgroups are enabled 
  See for example: https://microk8s.io/docs/install-alternatives#heading--arm 
  Building the report tarball
    Report tarball is at /var/snap/microk8s/4055/inspection-report-20221017_162601.tar.gz
  ```

- Use kubernetes behind a proxy
As checked journalctl, found there's a connection problem that access https://k8s.gcr.io failed. So we need to setup a VPN or Shadowsocks connection before we can start use it.

	An example is using shadowsocks via host machine inside vm. Refer to [using shadowsocks via host machine](/en/linux/using-shadow-socks-via-host-machine) for more information.

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
