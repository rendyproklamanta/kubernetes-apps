## Deploy traefik, SSL and sample app

- Install Traefik
```
kubectl create namespace traefik
helm repo add traefik https://traefik.github.io/charts
helm repo update
helm upgrade --install traefik traefik/traefik -n traefik -f traefik/values.yaml
```

- Install cert-manager (if none)
```
kubectl create namespace cert-manager
kubectl apply --validate=false -f https://github.com/cert-manager/cert-manager/releases/download/v1.13.0/cert-manager.crds.yaml
helm template cert-manager jetstack/cert-manager --namespace cert-manager --version v1.13.0 | kubectl apply -f -
```

- Install SSL
```
kubectl apply -k ssl
```

- Install Plugins (optionals)
```
kubectl create ns crowdsec
kubectl apply -f traefik/plugins.yml
helm repo add crowdsec https://crowdsecurity.github.io/helm-charts
helm repo update
helm upgrade --install --namespace=crowdsec crowdsec crowdsec/crowdsec --values=./traefik/plugins/crowdsec/values.yml
```

### Deploy Sample app
```
kubectl apply -f deploy/certificate.yaml
kubectl apply -f deploy/deployment.yaml
kubectl apply -f deploy/ingress.yaml
```

### Get Logs
```
kubectl get events -n traefik --sort-by='.metadata.creationTimestamp'
kubectl --kubeconfig=docsavespeople.yaml get all -n traefik
```