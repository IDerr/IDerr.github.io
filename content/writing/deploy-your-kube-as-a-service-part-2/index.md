---
author: IDerr
title: Deploy your kubernetes as a service using Kubermatic ! Part 2 
date: 2022-08-11
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

In the last article, we saw the global architecture of Kubermatic, we will now deploy it on a single kubernetes cluster using Helm, without deploying all the stuff required by Kubermatic team

## Prerequisites

- A storage class named kubermatic-fast
- A kubernetes cluster for managing kubermatic and seed clusters

