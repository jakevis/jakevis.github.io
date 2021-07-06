---
title: "Deploying Ranger on Microk8s (on WSL2) with metallb and traefik"
#date: 2019-08-14T15:30:00-06:00
#last_modified_at: 2019-08-14T15:30:00-06:00
categories:
  - Blog
tags:
  - Linux
  - microk8s
  - kubernetes
---

I am running Windows 11, and have enabled WSL2 with Ubuntu and Microk8s. This will deploy Rancher.

* TOC
{:toc}

## Install microk8s plugins and correct privileges

```bash
microk8s.enable dns storage traefik helm3 metallb
```

Allow running priviledged Pods (required by Rancher's `cattle-node-agent`)
```bash
sudo sh -c 'echo "--allow-privileged=true" /var/snap/microk8s/current/args/kube-apiserver'
```

Restart:
```bash
sudo systemctl restart snap.microk8s.daemon-apiserver.service
```

Check the pods
```bash
microk8s.kubectl get pods --all-namespaces
```

## Install cert-manager
```bash

microk8s.kubectl apply --validate=false -f https://github.com/jetstack/cert-manager/releases/download/v1.0.4/cert-manager.crds.yaml
microk8s.kubectl create namespace cert-manager
microk8s.helm3 repo add jetstack https://charts.jetstack.io
microk8s.helm3 repo update
microk8s.helm3 install \
  cert-manager jetstack/cert-manager \
  --namespace cert-manager \
  --version v1.0.4

```

## Install Rancher
```bash
microk8s.kubectl create namespace cattle-system
microk8s.helm3 install rancher rancher-latest/rancher \
  --namespace cattle-system \
  --set hostname=rancher.woofy.io \
  --set replicas=1
```

Wait for it to be ready:
```bash
microk8s.kubectl -n cattle-system rollout status deploy/rancher
```

## Expose Rancher

```bash
microk8s.kubectl expose deployment rancher -n cattle-system --type=LoadBalancer --name=rancher-lb --port=443
```
Get details:
```bash
microk8s.kubectl get svc -n cattle-system -o wide
```

