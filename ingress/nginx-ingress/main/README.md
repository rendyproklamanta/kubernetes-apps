```
kubectl --kubeconfig=D:/kubeconfig/vultr/test.yaml apply -f nginx-ingress.yaml
```

```
kubectl --kubeconfig=D:/kubeconfig/vultr/test.yaml apply -f https://github.com/jetstack/cert-manager/releases/download/v1.7.0/cert-manager.yaml
```

```
kubectl --kubeconfig=D:/kubeconfig/vultr/test.yaml apply -f letsencrypt.yaml
```
