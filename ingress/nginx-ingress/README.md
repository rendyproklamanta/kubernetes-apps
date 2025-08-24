# Vultr Kubernetes Engine

Ref : https://www.vultr.com/docs/how-to-deploy-a-next-js-application-with-vultr-kubernetes-engine-and-vultr-load-balancer/

<br>

> First create directory "kubernetes" and move dir "app" & "main" inside

> Open dir deployer and select desired registry > then move to root directory

<br>

Goto : https://my.vultr.com/kubernetes/manage/ -> Click Instance -> Download Configuration

Move file vke .yaml and rename to :

```
D:\kubeconfig\vultr\test.yaml
```

## Setup Auto create Loadbalancer + SSL (One-time Only)

```
$ kubectl --kubeconfig=D:\kubeconfig\vultr\test.yaml apply -f main/nginx-ingress.yaml

$ kubectl --kubeconfig=D:\kubeconfig\vultr\test.yaml get ingress
```

```
$ kubectl --kubeconfig=D:\kubeconfig\vultr\test.yaml apply -f https://github.com/jetstack/cert-manager/releases/download/v1.7.0/cert-manager.yaml

$ kubectl --kubeconfig=D:\kubeconfig\vultr\test.yaml apply -f kubernetes/main/letsencrypt.yaml
```

## Deploy App

```
$ kubectl --kubeconfig=D:\kubeconfig\vultr\test.yaml apply -f kubernetes/app/app-deploy.yaml

$ kubectl --kubeconfig=D:\kubeconfig\vultr\test.yaml get pods
```

```
$ kubectl --kubeconfig=D:\kubeconfig\vultr\test.yaml apply -f kubernetes/app/app-ingress.yaml

$ kubectl --kubeconfig=D:\kubeconfig\vultr\test.yaml get ingress
```

```
$ kubectl --kubeconfig=D:\kubeconfig\vultr\test.yaml apply -f kubernetes/app/app-autoscalling.yaml
```

<br>

## Deploy to GitLab or Other

Go to directory <b>deployer/</b> <br>
GitLab : Go to directory <b>deployer/gitlab</b> <br>
And see README.md

<br>

## Exec pods

```
kubectl --kubeconfig=D:\kubeconfig\vultr\test.yaml get pods
```

```
kubectl --kubeconfig=D:\kubeconfig\vultr\test.yaml exec --stdin --tty <pods-name-xxxx> -- /bin/bash
```

<br>

## Rolling update kubernetes

```
kubectl --kubeconfig=D:\kubeconfig\vultr\test.yaml rollout restart deployment <app-name>
```

<br>

## Delete Data

Delete Everything

```
kubectl --kubeconfig=D:\kubeconfig\vultr\test.yaml delete --all all
```

Delete Ingress

```
$ kubectl --kubeconfig=D:\kubeconfig\vultr\test.yaml get ingress
$ kubectl --kubeconfig=D:\kubeconfig\vultr\test.yaml delete ingress <app-ingress>
```

Delete Deployment

```
kubectl --kubeconfig=D:\kubeconfig\vultr\test.yaml delete deployment <app-deployment-name>
```

Delete Service

```
$ kubectl --kubeconfig=D:\kubeconfig\vultr\test.yaml get svc
$ kubectl --kubeconfig=D:\kubeconfig\vultr\test.yaml delete service <app-service>
```

## COMMANDS

```
$ kubectl --kubeconfig=D:\kubeconfig\vultr\test.yaml get all
$ kubectl --kubeconfig=D:\kubeconfig\vultr\test.yaml get pods
$ kubectl --kubeconfig=D:\kubeconfig\vultr\test.yaml get ingress
$ kubectl --kubeconfig=D:\kubeconfig\vultr\test.yaml describe pod <pod_name>
```
