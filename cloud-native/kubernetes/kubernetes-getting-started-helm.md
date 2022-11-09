---
title: Getting started of k8s - helm
description: 
published: true
date: 2022-11-09T05:09:20.180Z
tags: 
editor: markdown
dateCreated: 2022-10-31T05:04:51.391Z
---

# Getting started of Helm

## Use microk8s helm3 instead

If you have microk8s enabled, then you already have a built in helm addon to use.

You can create an alias in ~/.bash_aliases to avoid installing another instance of helm.
```bash
alias helm='microk8s helm3'
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

- Check connectivity

Check status of svc
```bash
kubectl get svc --namespace default happy-panda-wordpress
```

Check svc's ip address for external access
```bash
kubectl get svc --namespace default happy-panda-wordpress -o jsonpath="{.spec.clusterIP}"
```

- Enable Loadbalancer for microk8s
```bash
microk8s enable metallb
```