---
title: Getting started of kubenetes - helm
description: 
published: true
date: 2022-10-31T05:05:53.300Z
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