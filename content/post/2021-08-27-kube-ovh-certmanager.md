---
title: "Use OVH as a DNS-01 provider for cert-manager"
subtitle: ""
date: 2021-08-27
author: "IDerr"
image: "https://cdna.artstation.com/p/assets/images/images/009/211/358/large/bala-vidhya-sagar-1.jpg?1517728575"
published: true
tags:
    - Kubernetes
    - Cert-Manager
    - OVH
categories: 
  - Tech
---

## Introduction
First article of this new blog !

Today we will discuss how to configure automatic Let’s-Encrypt certificate renewal with a domain hosted in OVH.

I have not found a clear tutorial on how to setup a cluster wide OVH cert-manager provider so there it is.

## Installation
### Cert-manager installation

Quick reminder, installing cert-manager is pretty straight forward with Helm
Don't forget to replace the version with the latest one : https://github.com/jetstack/cert-manager/releases
```bash
kubectl create namespace cert-manager
helm repo add jetstack https://charts.jetstack.io
helm repo update
helm install cert-manager jetstack/cert-manager --namespace cert-manager --create-namespace --version v1.5.3 --set installCRDs=true

```
After that, you should have a running cert-manager

### OVH Webhook installation

```bash
git clone https://github.com/baarde/cert-manager-webhook-ovh.git
cd cert-manager-webhook-ovh
helm install cert-manager-webhook-ovh ./deploy/cert-manager-webhook-ovh --set groupName='<GROUP_NAME>'
```
After that, we need to create our api keys in the OVH API to connect our webhook controller to OVH

- Go to https://api.ovh.com/createToken/index.cgi 
- Add the followings rights, if you want to give acces to all of your domains
  - GET /domain/zone/*
  - PUT /domain/zone/*
  - POST /domain/zone/*
  - DELETE /domain/zone/*
- If you prefer to give access only to one domain replace the "*" by your domain name

We'll store the freshly generated application secret in Kubernetes.

The secret needs to be in the same namespace as the cert-manager controller pod if you want to create a ClusterIssuer, in our case, 'cert-manager'

```bash
kubectl create secret generic ovh-credentials --namespace cert-manager --from-literal=applicationSecret='<OVHSECRET>'
```
Grant permission to get the secret to the cert-manager-webhook-ovh service account
```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  name: cert-manager-webhook-ovh:secret-reader
rules:
- apiGroups: [""]
  resources: ["secrets"]
  resourceNames: ["ovh-credentials"]
  verbs: ["get", "watch"]
---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: cert-manager-webhook-ovh:secret-reader
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cert-manager-webhook-ovh:secret-reader
subjects:
- apiGroup: ""
  namespace: default
  kind: ServiceAccount
  name: cert-manager-webhook-ovh
```
And we can finally create our cluster issuer, don't forget to replace the values between <> with your keys/config
```yaml
apiVersion: cert-manager.io/v1
kind: ClusterIssuer
metadata:
  name: letsencrypt
  namespace: cert-manager
spec:
  acme:
    server: https://acme-v02.api.letsencrypt.org/directory
    email: '<EMAIL>'
    privateKeySecretRef:
      name: letsencrypt-account-key
    solvers:
    - dns01:
        webhook:
          groupName: '<GROUP_NAME>'
          solverName: ovh
          config:
            endpoint: ovh-eu
            applicationKey: '<APP_KEY>'
            applicationSecretRef:
              key: applicationSecret
              name: ovh-credentials
            consumerKey: '<CONSUMER_KEY>'
```
And voila, you have a fully working ClusterIssuer with OVH, you can test all your work with a new Certificate.

```yaml
apiVersion: cert-manager.io/v1
kind: Certificate
metadata:
  name: example-certificate
spec:
  dnsNames:
  - test.mydomain.com
  issuerRef:
    name: letsencrypt
    kind: ClusterIssuer
  secretName: test-mydomain-com-tls
```

```bash
NAME                READY   SECRET                  AGE
example-certificate True    test-mydomain-com-tls   3s
```

## Conclusion

Congrats, and see you next time for another article ! 