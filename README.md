# Introduction

This repository contains files to setup Rancher on the Google Kubernetes Engine.

# Commands

Install `ingress-nginx`:
```bash
helm upgrade --install --wait --debug \
    ingress-nginx ingress-nginx/ingress-nginx \
    -n infrastructure \
    -f infrastructure/ingress-nginx/values.yaml \
    --create-namespace
```

Install `cert-manager`:
```bash
helm upgrade --install --wait --debug cert-manager \
    -f infrastructure/cert-manager/values.yaml \
    -n infrastructure \
    jetstack/cert-manager
```

Install `rancher`:
```bash
helm install rancher rancher-latest/rancher \
    -f infrastrucutre
    --namespace cattle-system \
    --set bootstrapPassword=***
```