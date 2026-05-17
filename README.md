---
Description: This template builds a Java app on Azure Kubernetes Service (AKS) cluster using the docker image built and pushed to container registry.
Products: 
- Azure Portal
- Azure CLI
- Docker
Language: 
- Java
- YAML
---
# Create an AKS Java app (OpenJDK 21 Ubuntu,Spring Boot), docker image and container registry

# Overview & Deployed resources
## Overview
This template creates a java app on AKS cluster that streamlines app deployment without worrying about underlying server infrastructure. The solution deploys an AKS cluster with default node pool and node count 1. It then builds a docker image using a sample java app and docker files, and is pushed to a container registry. Using a Kubernetes deployment file, it deploys the app to AKS cluster.

## Resources
### Create a container registry
```sh
az acr create --resource-group <rg name> --name <registry name> --sku Premium
az acr login --name <registry name>
```

### Build & Push the docker image
```sh
docker build -t aks-java-sample:latest .
docker image push <container registry>/aks-java-sample:latest
```

### Create & Deploy to AKS
```sh
az aks create --resource-group <rg name> --name <cluster name> --node-count 1 --enable-addons monitoring --generate-ssh-keys --attach-acr <registry name>
az aks get-credentials --resource-group <rg name> --name <cluster name>

kubectl apply -f deployment.yaml
kubectl get svc aks-java-sample-service // Access via the EXTERNAL-IP
```
Open `http://<EXTERNAL-IP>/` to see the greeting.

`Tags: Container Registry, Docker, Dockerfile, Docker image, AKS, Kubectl, Java`
