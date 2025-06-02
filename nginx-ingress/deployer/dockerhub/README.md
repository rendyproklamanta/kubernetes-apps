## Create DockerHub credential

```
kubectl --kubeconfig=D:\kubeconfig\vultr\test.yaml create secret docker-registry dockerhub-token --docker-username=<USERNAME> --docker-password=<PASSWORD> --docker-email=<EMAIL>
```

## Build and push to DockerHub

```
docker build . -t <MY_DOCKERHUB_USERNAME>/<repositoryname>
docker tag app-name <MY_DOCKERHUB_USERNAME>/<repositoryname>:latest
docker push <MY_DOCKERHUB_USERNAME>/<repositoryname>:latest
```

## Add variables to repository
```
DOCKERHUB_USERNAME = mydockerhubusername
DOCKERHUB_PASSWORD = mydockerhubpassword
DOCKERHUB_REGISTRY = docker.io
DOCKERHUB_REGISTRY_IMAGE = mydockerhubusername/<repositoryname>
```