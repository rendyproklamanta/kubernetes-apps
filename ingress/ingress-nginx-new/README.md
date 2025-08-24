helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
helm repo update

helm upgrade --install ingress-nginx ingress-nginx/ingress-nginx \
  --namespace ingress-nginx --create-namespace \
  -f nginx/values.yaml


kubectl apply -f nginx/cert.yaml
kubectl apply -f nginx/deployment.yaml
kubectl delete -f nginx/deployment.yaml


kubectl get pods
kubectl get pod -n ingress-nginx


helm uninstall ingress-nginx \
  --namespace ingress-nginx