## Create secret db password :
```
kubectl --kubeconfig=D:/kubeconfig/vultr/test.yaml create secret generic mysql-pass --from-literal=password=YOUR_PASSWORD
kubectl --kubeconfig=D:/kubeconfig/vultr/test.yaml get secrets
```

## Create PersistentVolume :
```
kubectl --kubeconfig=D:/kubeconfig/vultr/test.yaml apply -f volumes.yaml
kubectl --kubeconfig=D:/kubeconfig/vultr/test.yaml get pv,pvc
```

## Deployment :
```
kubectl --kubeconfig=D:/kubeconfig/vultr/test.yaml apply -f deployment.yaml
```

```
kubectl --kubeconfig=D:/kubeconfig/vultr/test.yaml apply -f ingress.yaml
```