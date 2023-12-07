
# Fault-Tolerant Web Hosting on Kubernetes

This project covers the step by step guide to Containerize and Deploy Web Application using Docker, Kubernetes and Helm Charts.

## DevOps Tools / Service Used
+ Docker
+ Kubernetes
+ Kind

## Prerequisites
To deploy the Web Application on Docker, Kubernetes and Kind, we have the following prerequisites:

+ Install and configure [Docker](https://docs.docker.com/desktop/)

+ Install and configure [Kubernetes](https://kubernetes.io/docs/setup/)

+ Install and configure [Kind](https://kind.sigs.k8s.io/docs/user/quick-start/)

## How to work

We will start by containerizing our web application. You do not need to worry about creating an application and a Docker file.

I have already created my porfolio React application. This application is available inside the `Application` folder.

You also do not worry about creating a `Dockerfile`, as i have already prepared a `Dockerfile`, which is present inside the `Application` folder.

### Task 1: Containerising and Pushing our Application
+ Create a docker image
+ Push this image to Docker Hub

Use the `cd Application` command to change the directory to the application.

Use the `docker build -t username/my-image_name .` command to build your Docker image.

Use the following commands to push your image to Docker Hub.

```shell
docker login -u my-username -p my-password
docker push my-username/my-image-name
```

After building the image, you can use the following command to verify the image:

```shell
docker images
```

### Task 2: Set up a Cluster

To run Kubernetes, you need a cluster. A cluster is a group of computers working together that can be viewed as a single system.

Kubernetes provides us with several distributions to create this cluster. For learning purposes though, we do not need a complete cluster. Instead, we can use any one of the following:

+ minikube
+ kind
+ Docker Desktop
+ kubeadm

We’ll be using kind in the future to keep it simple.

+ Change your directory to the repo root.
+ Create a kind cluster.

We will use the following command: 

```shell
kind create cluster
```
### Task 3: Deploy the Service

Now we need to create our pods, deploy our application, and run the service to get started.

We need to add the image path in the `deployment.yaml` file , and then apply this files: 

+ deployment.yaml
+ service.yaml

We will use the following commands: 

```shell
kubectl apply -f deployment.yaml
kubectl apply -f service.yaml
```
### Task 4: Verify the state

To perform further operations, it is mandatory that our following resources are in the running state:

+ pods
+ deployment
+ service

Now we need to Check the status of all these resources, and make sure that our pods are running.

We will use the following commands: 
```shell
kubectl get pods
kubectl get deployment
kubectl get service
```
### Task 5: Access the Application

Now we need to Expose our application, and access the application from the browser.

To do that, we need to expose our application’s port.

We will use the following command: 
```shell
kubectl port-forward pods/your-pod-name --address 0.0.0.0 31111:3000
```

### Task 6: Add Fault-Tolerance

To make an application always accessible to the user, Kubernetes has a built-in mechanism that ensures the accessibility of pods at any given time.

In this step, we need to make sure that our pods are always running. For this purpose, our pod should contain a `restartPolicy spec` (already added).

we need to:

+ Check the running pods
+ Delete our pod
+ Check the status of our running pods

After deletion, a new pod will replace our initial pod.

We will use the `kubectl get pods` command to get the status of our pods.

Next, we will delete our pod.

Use the `kubectl delete pod your-pod-name` command to delete your pod.

### Task 9: Add an Ingress Controller

In Kubernetes, an ingress is a resource that manages the services within a cluster. In order to work, an ingress requires an ingress controller.

Following are some of the most commonly used Ingress controllers:

+ nginx Ingress Controller for Kubernetes
+ Kubernetes HAProxy ingress
+ GCP Ingress Controller

We will deploy the nginx ingress controller and Make sure that the controller is running

Use the following command to deploy the `nginx` ingress controller:
```shell
kubectl apply -f \
https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v0.44.0/\
deploy/static/provider/cloud/deploy.yaml
```

Use the following command to verify that the controller is running:

```shell
kubectl get pods --all-namespaces \
  -l app.kubernetes.io/name=ingress-nginx
```

### Task 10: Create new Resources

Before we move forward, we need to update our previously created resources, like pods, deployments, and services.

we need to:

+ Delete the previously created resources
+ Create new pods, deployment, and services
+ Check the status of the newly created resources

Use the following commands to delete the resources:

```shell
kubectl delete pod --all
kubectl delete services --all
kubectl delete deployment --all
```

Use the following commands to create new resources:

```shell
kubectl apply -f deployment.yaml
kubectl apply -f service.yaml
```

Use the following commands to check the status of the resources:

```shell
kubectl get pods
kubectl get deployment
kubectl get services 
```

### Task 11: Access our Application

To run the controller on the platform, we need to port-forward the ingress service.

we need to:


+ Use the `kubectl apply -f ingress.yaml` command to apply our ingress file
+ Use the `kubectl get ingress` command to check the status of our ingress
+ Use one of the following commands to port forward the service:
```shell
nohup kubectl port-forward --address 0.0.0.0 -n ingress-nginx service/ingress-nginx-controller 31111:80 > /dev/null 2>&1 &
```
```shell 
kubectl port-forward --address 0.0.0.0 -n ingress-nginx service/ingress-nginx-controller 31111:80
```
Here, `31111` is the port of the container, whereas our service is available to the outside world at port `80`.






















