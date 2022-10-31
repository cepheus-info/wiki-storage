---
title: Getting started of k8s - helm
description: 
published: true
date: 2022-10-31T06:26:05.796Z
tags: 
editor: markdown
dateCreated: 2022-10-31T05:04:51.391Z
---

# Getting started of Helm
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

## Using Helm

- Install package via helm command
  ```bash
  helm install happy-panda bitnami/wordpress
  ```
	This might cause an error like:
  ```bash
  Error: INSTALLATION FAILED: Kubernetes cluster unreachable: Get "http://localhost:8080/version": dial tcp 127.0.0.1:8080: connect: connection refused
  ```

- Resolve Kubernetes cluster unreachable error
  ```bash
  # If not using microk8s, omit [microk8s]
  microk8s kubectl config view --raw > ~/.kube/config
  ```
