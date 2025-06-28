# 🚀 Kubernetes Microservices Deployment with Minikube

## 📘 Project Overview

This project demonstrates the end-to-end deployment of a microservices-based Node.js application on Kubernetes using Minikube (Ubuntu environment). The system consists of four independently containerized microservices:

- **User Service** (`Port 3000`)
- **Product Service** (`Port 3001`)
- **Order Service** (`Port 3002`)
- **Gateway Service** (`Port 3003` - acts as API gateway)

The application is deployed locally using Minikube, and the services communicate internally via `ClusterIP` services.

---

## 📁 Project Structure

submission/
├── deployments/
│ ├── user-service.yaml
│ ├── product-service.yaml
│ ├── order-service.yaml
│ └── gateway-service.yaml
├── services/
│ ├── user-service.yaml
│ ├── product-service.yaml
│ ├── order-service.yaml
│ └── gateway-service.yaml
├── screenshots/
│ ├── pods.png
│ ├── logs.png
│ └── service-test.png
└── README.md


---

## 🧰 Prerequisites

Ensure the following tools are installed on your Ubuntu system:

- Docker
- kubectl
- Minikube

### Install Commands

```bash
sudo apt update
sudo apt install docker.io -y
curl -LO "https://dl.k8s.io/release/$(curl -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
chmod +x kubectl && sudo mv kubectl /usr/local/bin/
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube
```

## 🔥 Minikube Setup
```bash
minikube start --driver=docker --memory=4096 --cpus=2
eval $(minikube docker-env)
```

## 🐳 Build Docker Images (Inside Minikube Docker)
```bash
docker build -t user-service:latest ./user
docker build -t product-service:latest ./product
docker build -t order-service:latest ./order
docker build -t gateway-service:latest ./gateway
```
check image
```bash
docker image
```

## ⚙️ Kubernetes Deployment

### Apply Deployments
```bash
kubectl apply -f deployments/
```
### Apply Services
```bash
kubectl apply -f services/
```

## ✅ Verify Components

### View All Pods
```bash
kubectl get pods -o wide
```



