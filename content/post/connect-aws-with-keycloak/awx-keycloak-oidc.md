
---
author: IDerr
title: Connect your AWX/Ansible Tower with Keycloak !  
date: 2022-11-04
description: Connect your AWX/Ansible Tower with Keycloak  !
image: "background.jpg"
categories: 
    - "Keycloak"
    - "AWX"
    - "Ansible Tower"
tags:
    - Keycloak
    - AWX
    - Ansible Tower
---

## Introduction

Want to connect your AWX/Ansible Tower with your SSO solution (thanks to openid connect), in my case keycloak, you're in the right place.

## Prerequisites

    - A keycloak
    - An ansible tower / awx

If you respect all these prerequisites, you are good to go 

## Tutorial 

Add a client in keycloak with this redirect url
https://<AWX_HOST>/sso/complete/oidc/


add in awx/tower a generic oidc connection with client id / secret with this url
https://<KEYCLOAK_HOST>/realms/master


## Conclusion

Now that your awx/ansible tower is connected, login and do as you please! 

See you on the next article ! 
