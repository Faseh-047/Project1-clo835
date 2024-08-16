Full-Stack Application with Kubernetes
This repository hosts a full-stack application consisting of a frontend, backend, and MongoDB database, all deployed using Kubernetes. The stack includes a Node.js API server (backend), an Nginx server serving static web content (frontend), and a MongoDB database.

Table of Contents
Backend
Frontend
Kubernetes Configuration Files
Getting Started
Deploy to Kubernetes
Verify Deployments
Accessing the Application
1. Backend
Dockerfile: Contains the instructions to build the Docker image for the Node.js API server.
index.js: Main entry point for the Node.js API server, handling the application logic.
package.json: Configuration file managing dependencies for the Node.js application.
package-lock.json: Auto-generated file that locks the installed dependency versions.
2. Frontend
Dockerfile: Contains the instructions to build the Docker image for the Nginx server serving the static files.
index.html: Main HTML file for the frontend.
script.js: JavaScript file for adding interactive functionality to the frontend.
styles.css: Stylesheet file for designing the frontend.
3. Kubernetes Configuration Files
MongoDB

deployment.yaml: Defines the deployment strategy for MongoDB, including replica settings.
service.yaml: Exposes MongoDB as a service within the Kubernetes cluster using ClusterIP.
Nginx

deployment.yaml: Defines the deployment strategy for the Nginx server, including replica settings.
service.yaml: Exposes the Nginx server using NodePort to allow external access.
Node.js

deployment.yaml: Defines the deployment strategy for the Node.js API server, including replica settings.
service.yaml: Exposes the Node.js API server within the Kubernetes cluster.
configMap.yaml: Contains environment-specific configurations for the Node.js application, such as MongoDB connection strings.
Persistent Volumes

pv.yaml: PersistentVolume configuration for MongoDB to prevent data loss during pod restarts.
pvc.yaml: PersistentVolumeClaim for dynamically provisioning storage for MongoDB.
Secrets

secret.yaml: Securely stores sensitive data, such as MongoDB credentials, within Kubernetes.
ConfigMap

configMap.yaml: Stores environment-specific configurations for the Node.js application.
Getting Started
Prerequisites
Ensure the following tools are installed on your local machine:

Docker
Kubernetes (Minikube or any Kubernetes cluster)
kubectl
Build and Push Docker Images
Before deploying to Kubernetes, build and push the Docker images for the backend and frontend to Docker Hub:


# Backend
cd backend
docker build -t faseh047/node-backend .
docker push faseh047/node-backend

# Frontend
cd frontend
docker build -t faseh047/nginx-frontend .
docker push faseh047/nginx-frontend
Deploy to Kubernetes
Follow these steps to deploy the application to Kubernetes:


# Create the namespace
kubectl create namespace fullstack-app

# Deploy ConfigMap
kubectl apply -f deployment/configmaps.yaml -n fullstack-app

# Deploy Persistent Volume and Persistent Volume Claim
kubectl apply -f deployment/pv.yaml -n fullstack-app
kubectl apply -f deployment/pvc.yaml -n fullstack-app

# Deploy Secrets
kubectl apply -f deployment/secrets.yaml -n fullstack-app

# Deploy MongoDB
kubectl apply -f deployment/mongodb-deployment.yaml -n fullstack-app

# Deploy Node.js backend
kubectl apply -f deployment/backend-deployment.yaml -n fullstack-app

# Deploy Nginx frontend
kubectl apply -f deployment/frontend-deployment.yaml -n fullstack-app
Verify Deployments
Ensure all pods are running smoothly:


kubectl get pods -n fullstack-app
Accessing the Application
To access the frontend application, use the following commands:


kubectl get svc -n fullstack-app
minikube ip
Access the application using the NodePort exposed by the Nginx service:


http://<minikube-ip>:<node-port-nginx>