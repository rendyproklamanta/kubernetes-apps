# Github Deployer

Ref : https://event-driven.io/en/how_to_buid_and_push_docker_image_with_github_actions/

<br>

### ✅ Create GitHub Personal Access Token

1. https://github.com/settings/tokens
2. Generate New Token :

```
Name : GHCR_PAT
Expiration : No expiration
Select scopes:
- repo
- read:packages
- write:packages
```

3. Save token (note: token generated one-time only)

<br>

### ✅ Kubernetes set credential for github :

```
kubectl --kubeconfig=D:\kubeconfig\vultr\test.yaml create secret docker-registry github-token --docker-server=https://ghcr.io --docker-username=<MyGithubUsername> --docker-password=<MyGithubToken>
```

### ✅ Add secret :

1. Download VKE yml if use Vultr :
   https://my.vultr.com/kubernetes/manage/ -> Click Instance -> Download Configuration
2. Place to D:/kubeconfig/vultr/test.yaml
3. Convert to base64 :

```
cat D:/kubeconfig/vultr/test.yaml | base64
```

3. Copy all content

Go to secret url : https://github.com/{MyGithubUsername}/{MyGithubRepository}/settings/secrets/actions

```
GHCR_PAT : MyGithubToken
KUBE_CONFIG : Paste all content from "cat D:/kubeconfig/vultr/test.yaml | base64"
```

### ✅ Change value in yml :

Open production.yml :

```
> tags: ghcr.io/<MyGithubUsername>/<MyGithubRepository>:latest
> Change app-deploy-name
```

Open kubernetes/app/app-deploy.yml

```
image: ghcr.io/<MyGithubUsername>/<MyGithubRepository>:latest
```
