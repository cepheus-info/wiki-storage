---
title: Kubernetes Highly Available Clusters
description: 
published: true
date: 2022-11-10T06:58:00.096Z
tags: 
editor: markdown
dateCreated: 2022-11-10T03:58:47.798Z
---

# Kubernetes Highly Available Clusters

## Overview

We can setup kubernetes clusters with stacked control plane nodes. This approach requires less infrastructure. The etcd members and control plane nodes are co-located.

The following diagram shows the architecture of a Kubernetes cluster with stacked control plane nodes.
![HA with Stacked Control Plane Topology](/assets/kubernetes-cluster-stacked-control-plane-diagram.png)

## Prerequisites

At least 3 nodes are required to setup a Kubernetes cluster with stacked control plane nodes. The following table shows the minimum requirements for each node.

- A compatible Linux host.
- 2 GB or more of RAM per machine (any less will leave little room for your apps).
- 2 CPUs or more.
- Full network connectivity between all machines in the cluster (public or private network is fine).
- Unique hostname, MAC address, and product_uuid for every node.
  ```bash
  # Get the MAC address of the network
  ip link or ifconfig -a
  ```
  ```bash
  # Check the product_uuid
  sudo cat /sys/class/dmi/id/product_uuid
  ```
- Certain ports are open on your machines.

  ```bash
  # The port 6443 is used for Kubernetes API server to communicate with the kubelet processes on each node.
  nc 127.0.0.1 6443
  ```

- Swap disabled. You MUST disable swap in order for the kubelet to work properly.

  ```bash
  # Turn off swap
  sudo swapoff -a
  # Check if swap is disabled
  sudo swapon --show
  ```

## Setup Kubernetes HA Cluster

### Install Container Runtime

We can use containerd as the container runtime for Kubernetes. We can install containerd by following the steps in the [Getting started with containerd](/en/cloud-native/kubernetes/install-containerd) document.

### Install Kubernetes Components

- [Install kubeadm](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/)
- [Install kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/)
- [Install kubelet](https://kubernetes.io/docs/tasks/tools/install-kubelet/)
- [Install docker](https://docs.docker.com/engine/install/)
