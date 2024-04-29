# csi-secret-store

## Install Vault 
```
helm repo add hashicorp https://helm.releases.hashicorp.com
helm search repo hashicorp/vault
helm search repo hashicorp/vault --versions
helm install vault hashicorp/vault \
    --set "server.dev.enabled=true" \
    --set "injector.enabled=false" \
    --set "csi.enabled=true"
```
## exec into the pod
```
kubectl exec -it vault-0 -- /bin/sh
```

## Create the KV store and secret from the Vault UI or CLI 
```
vault kv put secret/my-pass password="kubernetes"
vault kv get secret/my-pass
```
## Enable Kubernetes Authentication
```
vault auth enable kubernetes
vault write auth/kubernetes/config kubernetes_host="https://172.30.1.2:6443" // this is for Killercoda

```
## Create a policy
```
vault policy write read-only - <<EOF
path "secret/data/my-pass" {
  capabilities = ["read"]
}
EOF
```
## Binding policy with Kubernetes SA
```
vault write auth/kubernetes/role/mysecret \
    bound_service_account_names=secret-sa \
    bound_service_account_namespaces=default \
    policies=read-only \
    ttl=20m
```

## Create service account 
```
kubectl create serviceaccount secret-sa
```

## Install csi-secret-store
```
helm repo add secrets-store-csi-driver https://kubernetes-sigs.github.io/secrets-store-csi-driver/charts
helm install csi-secrets-store secrets-store-csi-driver/secrets-store-csi-driver --namespace kube-system
```
## Create the secret provider class
kubectl apply -f secretproviderclass

## create the pod 
kubectl apply -f pod.yaml


## [Customize installation](https://github.com/kubernetes-sigs/secrets-store-csi-driver/tree/main/charts/secrets-store-csi-driver#configuration)
```
enableSecretRotation	Enable secret rotation feature [alpha]	false
yncSecret.enabled	Enable rbac roles and bindings required for syncing to Kubernetes native secrets	false
```
