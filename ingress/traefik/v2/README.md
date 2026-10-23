## Deploy traefik & SSL

# 1. Install cert-manager (skip if already installed in vultr)
```
kubectl create namespace cert-manager
kubectl apply --validate=false -f https://github.com/cert-manager/cert-manager/releases/download/v1.13.0/cert-manager.crds.yaml
helm template cert-manager jetstack/cert-manager --namespace cert-manager --version v1.13.0 | kubectl apply -f -
```

# 2. Configure SSL
```
kubectl --kubeconfig=kubeconfig.yaml apply -k ssl
```

## Deploy Traefik v2

- Create namespace
```
kubectl --kubeconfig=kubeconfig.yaml create namespace traefik
```

- Add repo Helm Traefik
```
helm repo add traefik https://traefik.github.io/charts
helm repo update
```

- Install/upgrade Traefik menggunakan `traefik/values.yaml`
  > Chart `traefik/traefik` terbaru sudah untuk Traefik v3 dan skema values-nya tidak cocok dengan `values.yaml` di sini (pakai `ports.*.redirectTo`, `ports.*.tls`, `logs`, dll). Pin ke chart `27.0.2` (kompatibel dengan app v2.11.x).
```
helm upgrade --install traefik traefik/traefik -n traefik -f traefik/values.yaml --version 27.0.2 --kubeconfig=kubeconfig.yaml
```

- Cek status deployment
```
kubectl --kubeconfig=kubeconfig.yaml get pods -n traefik
kubectl --kubeconfig=kubeconfig.yaml get svc -n traefik
```

# Install Plugins (optionals)
```
kubectl create ns crowdsec
kubectl apply -f traefik/plugins.yml
helm repo add crowdsec https://crowdsecurity.github.io/helm-charts
helm repo update
helm upgrade --install --namespace=crowdsec crowdsec crowdsec/crowdsec --values=./traefik/plugins/crowdsec/values.yml
```

### Deploy Sample app
- Create directory k8s/production|staging
- Create kustomization.yaml, and inside dir :
```
certificate.yaml
deployment.yaml
ingress.yaml
```
- Deploy app
```
kubectl apply -k k8s/production
```
