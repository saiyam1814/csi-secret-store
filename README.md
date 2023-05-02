# csi-secret-store

## Install Vault 
```
kubectl create namespace vault
helm repo add hashicorp https://helm.releases.hashicorp.com
helm search repo hashicorp/vault
helm search repo hashicorp/vault --versions
helm install vault hashicorp/vault --namespace vault --version 0.5.0
```
## edit service to nodeport
```
kubectl edit svc -n vault vault-internal
```

## Create the KV store and secret from the Vault UI

## Install csi-secret-store
```
helm repo add secrets-store-csi-driver https://kubernetes-sigs.github.io/secrets-store-csi-driver/charts
helm install csi-secrets-store secrets-store-csi-driver/secrets-store-csi-driver --namespace kube-system
```
## [Customize installation](https://github.com/kubernetes-sigs/secrets-store-csi-driver/tree/main/charts/secrets-store-csi-driver#configuration)
```
enableSecretRotation	Enable secret rotation feature [alpha]	false
yncSecret.enabled	Enable rbac roles and bindings required for syncing to Kubernetes native secrets	false
```
