# How to deploy to Kubernetes a .NET 8 Web API

## 1. Prerequisites

Download and Install Docker Desktop

Run Docker Desktop

Navigate to

## 2. Summarizing the steps for deploying your application to Kubernetes

Here are the general steps to deploy your .NET 8 Web API to Kubernetes:

1. Build and Push the Docker image to the Docker Hub registry/repo

2. Create Kubernetes Deployment YAML file. This file defines how your application is deployed in Kubernetes.

3. Create Kubernetes Service YAML file. This file defines how your application is exposed, either within Kubernetes cluster or to the outside world.

4. Apply the YAML files to your Kubernetes Cluster: use the command "**kubectl apply**" to create the resource defined in your YAML file in your Kubernetes cluster.

## 3. Build and Push the Docker image to the Docker Hub registry/repo



## 4. Create Kubernetes Deployment YAML file

**deployment.yaml**

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: webapi-deployment
spec:
  replicas: 2
  selector:
    matchLabels:
      app: webapi
  template:
    metadata:
      labels:
        app: webapi
    spec:
      containers:
      - name: webapi
        image: luiscoco/webapi:latest  # Replace with your image path
        ports:
        - containerPort: 8080
```

## 5. Create Kubernetes Service YAML file

**service.yaml**

```yaml
apiVersion: v1
kind: Service
metadata:
  name: webapi-service
spec:
  type: LoadBalancer
  ports:
  - port: 80
    targetPort: 8080
  selector:
    app: webapi
```
