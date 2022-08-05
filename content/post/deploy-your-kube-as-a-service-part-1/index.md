---
author: IDerr
title: Deploy your kubernetes as a service using Kubermatic ! 
date: 2022-08-03
description: How to Deploy your kubernetes as a service using Kubermatic !
image: "background.jpg"
categories: 
    - "Kubermatic"
    - "Kubernetes"
tags:
    - Kubernetes
    - Cloud
---

## Introduction

Want to build your own EKS, GKE on vanilla Kubernetes ? Here's an easy way to use Kubermatic as your kubernetes provider on your infra.

Kubermatic will manage all your kubernetes clusters using your favorite hypervisor, with a great multi-tenant UI.

## Prerequisites

To make your own Kubernetes as a Service you need : 
  - At least one kubernetes cluster, will be used to host control plane services and kubermatic software

  - At least one infrastructure provider (https://github.com/kubermatic/machine-controller)
    - AWS
    - GCP
    - Openstack
    - Hetzner 
    - Kubevirt
    - Digitalocean
    - Azure
    - Nutanix
    - VMWare Cloud Director
    - VMWare Vsphere
    - Linode
  Using one of the following operating system (Full compatibility here : https://github.com/kubermatic/machine-controller/blob/master/docs/operating-system.md): 
    - Ubuntu 
    - Flatcar 
    - CentOS 7

If you respect all these prerequisites, you are good to go 

## Architecture

Kubermatic has a 3 layer architecture : Master 
  - Master Cluster
  - Seed Cluster
  - User Cluster

We will go through each of them to detail their usage
### Master Cluster

Master cluster is reponsible for running all kubermatic related software :
  - Web UI aka Dashboard
  - Kubermatic API
  - Machine Controller responsible to deploy and manage worker nodes

### Seed Cluster


  



