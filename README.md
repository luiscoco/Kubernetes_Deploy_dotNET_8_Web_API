# How to deploy to Kubernetes a .NET 8 Web API

## 1. Prerequisites

Download and Install Docker Desktop: https://docs.docker.com/desktop/install/windows-install/

Run Docker Desktop

![image](https://github.com/luiscoco/Kubernetes_Deploy_dotNET_8_Web_API/assets/32194879/d7dc6eec-9505-44e4-966e-8bc2a1c37860)

Navigate to enable the Kubernetes option in Docker Desktop:

![image](https://github.com/luiscoco/Kubernetes_Deploy_dotNET_8_Web_API/assets/32194879/88f781ca-228b-4b68-b439-8237cb26c3d9)

![image](https://github.com/luiscoco/Kubernetes_Deploy_dotNET_8_Web_API/assets/32194879/2939ab9f-883e-4ef4-a9b1-ed436a8fc1e6)

![image](https://github.com/luiscoco/Kubernetes_Deploy_dotNET_8_Web_API/assets/32194879/1c7dd4f5-e503-46d8-aa93-c0d2d383fa22)

![image](https://github.com/luiscoco/Kubernetes_Deploy_dotNET_8_Web_API/assets/32194879/7206c4e6-fc89-4b76-b9fd-d93b61f7c7a9)

![image](https://github.com/luiscoco/Kubernetes_Deploy_dotNET_8_Web_API/assets/32194879/35f0cd42-7d35-4ee3-b0d9-e815522947f6)

## 2. Summarizing the steps for deploying your application to Kubernetes

Here are the general steps to deploy your .NET 8 Web API to Kubernetes:

1. Build and Push the Docker image to the Docker Hub registry/repo

2. Create Kubernetes Deployment YAML file. This file defines how your application is deployed in Kubernetes.

3. Create Kubernetes Service YAML file. This file defines how your application is exposed, either within Kubernetes cluster or to the outside world.

4. Apply the YAML files to your Kubernetes Cluster: use the command "**kubectl apply**" to create the resource defined in your YAML file in your Kubernetes cluster.

## 3. Build and Push the Docker image to the Docker Hub registry/repo

For more details about this section see the repo: https://github.com/luiscoco/Docker_Create_and_run_Image-_for_dotNET_8_Web_API

```
docker build -t luiscoco/webapidotnet8:latest .
```

Then we use the docker push command to upload the image to the Docker Hub repository:

```
docker push luiscoco/webapidotnet8:latest
```

## 4. Create Kubernetes Deployment YAML file

In Visual Studio we add a new yaml file to our project

![image](https://github.com/luiscoco/Kubernetes_Deploy_dotNET_8_Web_API/assets/32194879/d17651ef-f1b8-42d8-86fd-f6db6c0c3438)

![image](https://github.com/luiscoco/Kubernetes_Deploy_dotNET_8_Web_API/assets/32194879/8673210a-2007-458d-93a6-e11a877008bb)


This is the source code for the **deployment.yaml** file:

**deployment.yml**

```yml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: webapidotnet8-deployment
spec:
  replicas: 2
  selector:
    matchLabels:
      app: webapidotnet8
  template:
    metadata:
      labels:
        app: webapidotnet8
    spec:
      containers:
      - name: webapidotnet8
        image: luiscoco/webapidotnet8:latest  # Replace with your image path
        ports:
        - containerPort: 8080
```

## 5. Create Kubernetes Service YAML file

In Visual Studio we add a new yaml file to our project. See the steps explained in the section 4.

![image](https://github.com/luiscoco/Kubernetes_Deploy_dotNET_8_Web_API/assets/32194879/110c1a8b-60cb-482f-b844-715acd427f72)

**service.yml**

```yml
apiVersion: v1
kind: Service
metadata:
  name: webapidotnet8-service
spec:
  type: LoadBalancer
  ports:
  - port: 80
    targetPort: 8080
  selector:
    app: webapidotnet8
```

## 6. Applying the YAML Files

```
kubectl apply -f deployment.yml
```

![image](https://github.com/luiscoco/Kubernetes_Deploy_dotNET_8_Web_API/assets/32194879/fb9d3322-2c2c-40e1-8a82-c60afd2c91b4)

```
kubectl apply -f service.yml
```

![image](https://github.com/luiscoco/Kubernetes_Deploy_dotNET_8_Web_API/assets/32194879/2309e706-36e1-4667-949a-29aadf712a18)

We can use the command "**kubectl get services**" to check the IP and port your application is accessible on, if using a LoadBalancer.

Verify the Deployment with the command:

```
kubectl get deployments
```

![image](https://github.com/luiscoco/Kubernetes_Deploy_dotNET_8_Web_API/assets/32194879/1b6e279d-ab83-43f4-b074-fd5e119667ab)

Verify the service status with the command:

```
kubectl get services
```

![image](https://github.com/luiscoco/Kubernetes_Deploy_dotNET_8_Web_API/assets/32194879/f90bf5fb-16e7-43b0-a771-ebdcb178c115)

## 7. Navigate to the Web API endpoint

http://localhost/weatherforecast

![image](https://github.com/luiscoco/Kubernetes_Deploy_dotNET_8_Web_API/assets/32194879/123fc551-fc57-4e60-8baa-4bb129007ed7)


