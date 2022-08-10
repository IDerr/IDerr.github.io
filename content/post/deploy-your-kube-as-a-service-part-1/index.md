---
author: IDerr
title: Deploy your kubernetes as a service using Kubermatic ! Part 1  
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

Seed cluster is reponsible for running all control planes software for user clusters
Each user cluster will use a different namespace in the seed cluster to separate resources from each cluster.

### User Cluster

The user cluster is the cluster created for a tenant.

## Architecture

### Simple architecture

You'll need one kubernetes cluster for this simple architecture, master and seed cluster will be grouped in one cluster.
The user cluster will be instanciated in your provider.

![simple](https://d33wubrfki0l68.cloudfront.net/24dced1937561af303108d13284e6766df6be253/cd0c4/img/kubermatic/v2.20/architecture/combined-master-seed.png)
  
### Large scale architecture

You'll need at least two kubernetes clusters for this.
Master will be hosted in a cluster and the seed cluster in another. You can separate your clusters in multiple seeds depending on the granularity needed (Country/Datacenter or other)

The user cluster will still be instanciated in your provider.


![](https://d33wubrfki0l68.cloudfront.net/3c0b0b49415d5ece57c75e678ca15ac8edd8cea8/e39bb/img/kubermatic/v2.20/architecture/dedicated-seeds.png)