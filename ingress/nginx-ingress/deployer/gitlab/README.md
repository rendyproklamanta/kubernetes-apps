# Gitlab Deployer

Ref: https://cylab.be/blog/112/continuous-deployment-with-gitlab-and-kubernetes

<br>

### Deploy gitlab-role

```
kubectl --kubeconfig=D:\kubeconfig\vultr\test.yaml apply -f role.yaml
kubectl --kubeconfig=D:\kubeconfig\vultr\test.yaml apply -f role-binding.yaml

```

### ✅ Create gitlab service-account access

```
kubectl --kubeconfig=D:\kubeconfig\vultr\test.yaml create sa gitlab
```

```
kubectl --kubeconfig=D:\kubeconfig\vultr\test.yaml get sa gitlab -o yaml
```

> This will show gitlab-token-xxxxx

<br>

### ✅ Get token for [K8S_TOKEN]

```
kubectl --kubeconfig=D:\kubeconfig\vultr\test.yaml get secret gitlab-token-xxxxx -o yaml | findstr token:
```

### ✅ Get server for [K8S_SERVER]

```
kubectl --kubeconfig=D:\kubeconfig\vultr\test.yaml cluster-info | findstr "Kubernetes master"
```

<br>

## ✅ Settings > CI / CD / Variables:

K8S_TOKEN with the token that we just extracted
<br>
K8S_SERVER with the address of the kubernetes API server (https://kube.example.com:6443)

### ✅ Kubernetes access Tokens to GitLab :

To allow access from Kubernetes to the GitLab registry navigate to
<br>
Personal menu > Settings > Access Tokens
<br>
and create a Personal Access Token => Checked <b>[✓] api</b>.

### ✅ Kubernetes set credential :

```
$ kubectl --kubeconfig=D:\kubeconfig\vultr\test.yaml create secret docker-registry gitlab-token --docker-server=registry.gitlab.com --docker-username=<GITLAB_USERNAME> --docker-password=<GITLAB_TOKEN>
```

### ✅ Copy gitlab-ci.yml to root folder
