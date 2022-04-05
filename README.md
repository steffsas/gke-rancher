# Introduction

This repository contains files to setup Rancher on the Google Kubernetes Engine.

# Commands

To route incomming HTTP traffic, let's setup an `ingress-nginx` controller first.
```bash
helm upgrade --install --wait --debug \
    ingress-nginx ingress-nginx/ingress-nginx \
    -n infrastructure \
    -f infrastructure/ingress-nginx/values.yaml \
    --create-namespace
```

We want to secure all available applications using TLS, thus we install `cert-manager` that issues x509 certificates using LetsEncrypt automatically.
```bash
helm upgrade --install --wait --debug cert-manager \
    -n infrastructure \
    jetstack/cert-manager \
    --set installCRDs=true
```

The `ClusterIssuer` is the issuer noted in the ingress annoations to issue and inject the certificates to ingress resources.
```bash
kubectl apply \
    -f infrastructure/cert-manager/cluster-issuer.yaml \
    -n infrastructure
```

Let's setup `Rancher` to administrate the whole cluster. Adapt `bootstrapPassword` and `hostname` accordingly.
```bash
helm upgrade --install --wait --debug rancher rancher-latest/rancher \
    -f infrastructure/rancher/values.yaml
    --namespace cattle-system \
    --set bootstrapPassword=***
    --set hostname=***
```