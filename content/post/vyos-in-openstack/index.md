
---
author: IDerr
title: Use Vyos in Openstack!  
date: 2022-04-17
description: Use Vyos in Openstack!
image: "background.jpg"
categories: 
    - "VyOS"
    - "Openstack"
tags:
    - Openstack
    - VyOS
---

## Introduction

## Prerequisites

## Tutorial 
### Building VyOS image
```
git clone -b equuleus --single-branch https://github.com/vyos/vyos-build
cd vyos-build
docker run --rm -it --privileged --sysctl net.ipv6.conf.lo.disable_ipv6=0 -v $(pwd):/vyos -w /vyos vyos/vyos-build:equuleus bash
cd packages
git clone -b equuleus --single-branch https://github.com/vyos/vyos-1x
cd vyos-1x/
dpkg-buildpackage -uc -us -tc -b
cd ../../
./configure --architecture amd64 --build-by "XXXX@XX.XX" --build-type "release" 
sudo make iso
sudo make qemu
```


## Conclusion
