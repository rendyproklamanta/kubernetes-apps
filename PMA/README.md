Prerequisite : <br>
1. Set DNS
<br>
Add DNS A record "pma.domain.com"
<br>
Edit file ingress.yaml in "host/hosts" replace with your domain

2. Deploy ingress + ssl : <br>
Before deploy mysql and PMA. <br>
First deploy nginx-ingress and letsencrypt from here : <br>
https://github.com/rendyproklamanta/kubernetes-deployer/tree/main/main

```
$ kubectl --kubeconfig=D:\kubeconfig\vultr\test.yaml apply -f ingress.yaml
```

# Deploy PMA for 1 mysql instance/node :

```
$ kubectl --kubeconfig=D:\kubeconfig\vultr\test.yaml apply -f deployment.yaml
```

# Deploy PMA with percona cluster :

Ref => https://gist.github.com/rendyproklamanta/a8219be8517a25227908c4e7a0d9d925

To get root password from Percona Cluster :

```
$ kubectl --kubeconfig=D:\kubeconfig\vultr\test.yaml get secret db-cluster-secrets -o yaml
> root:XXXXXXXX

$ echo 'XXXXXXXX' | base64 --decode
> thisisrootpassword
```

Change "MYSQL_ROOT_PASSWORD" in deployment-cluster.yaml and set to : thisisrootpassword

- Deploy mysql-cluster :

```
$ kubectl --kubeconfig=D:\kubeconfig\vultr\test.yaml apply -f deployment-cluster.yaml
```
