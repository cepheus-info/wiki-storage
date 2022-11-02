---
title: Getting started of k8s
description: 
published: true
date: 2022-11-02T03:33:35.525Z
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
  echo 'export PATH="$PATH:/snap/bin"' >> ~/.bashrc
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
	As checked journalctl, found there's a connection problem that access https://k8s.gcr.io failed. In China, we need to setup a VPN or Shadowsocks connection before we can start use it.

	An example is using shadowsocks via host machine inside vm. Refer to [using shadowsocks via host machine](/en/linux/using-shadow-socks-via-host-machine) for more information.
  
  And next we need to configure http_proxy correctly in /etc/environment
```bash
HTTPS_PROXY=http://192.168.74.1:1080
HTTP_PROXY=http://192.168.74.1:1080
NO_PROXY=10.0.0.0/8,192.168.0.0/16,127.0.0.1,172.16.0.0/16
https_proxy=http://192.168.74.1:1080
http_proxy=http://192.168.74.1:1080
no_proxy=10.0.0.0/8,192.168.0.0/16,127.0.0.1,172.16.0.0/16
```

### Enable kubernetes dashboard addon
Note that a default access token is needed when log in to dashboard.
```bash
# create default access token
microk8s kubectl create token default
# Enable dashboard service
microk8s enable dashboard
# Show all kube services
microk8s kubectl get all --all-namespaces
```

### Forward port to host machine
```bash
microk8s kubectl port-forward -n kube-system service/kubernetes-dashboard 10443:443
```

### Use nginx to forward kubernetes-dashboard
As kubernetes-dashboard is using ssl, when we use a nginx server to proxy_pass https://localhost:10443, we should configure it like doc here: [configure-nginx-to-proxy-pass-https-site](/en/nginx/configure-nginx-to-proxy-pass-https-site).

## Install kubernetes-client
### Installation via package manager
```bash
sudo zypper install kubernetes1.24-client
```

### update kubectl config for microk8s
```bash
microk8s kubectl config view --raw > ~/.kube/config
```

## Enable hostpath-storage
The hostpath storage MicroK8s add-on can be used to easily provision PersistentVolumes backed by a host directory. 
```bash
microk8s enable hostpath-storage
```

## Alternative container engine
Sometimes we need to switch the used container engine of kubernetes. We can change below configuration:
```bash
vim /var/snap/microk8s/current/args/kubelet
```
	Change --container-runtime=docker from remote.
  
Then execute below command to restart microk8s.

```bash
microk8s stop

microk8s start
```
Note that Kubernetes supports container runtimes such as containerd, CRI-O, and any other implementation of the Kubernetes CRI (Container Runtime Interface).

