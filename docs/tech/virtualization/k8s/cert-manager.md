# CertManager

CertManager is a tool that use certbot to give https to all other kubernetes nodes.


```bash
helm repo add jetstack https://charts.jetstack.io --force-update
kubectl apply -f https://github.com/cert-manager/cert-manager/releases/download/v1.15.0/cert-manager.crds.yaml
```

Now we just need to install
```bash
helm install \
  cert-manager jetstack/cert-manager \
  --namespace cert-manager \
  --create-namespace \
  --version v1.15.0 
```
