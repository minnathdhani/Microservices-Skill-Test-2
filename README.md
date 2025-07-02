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

```
submission/
├── deployments/
│   ├── user-service.yaml
│   ├── product-service.yaml
│   ├── order-service.yaml
│   └── gateway-service.yaml
├── services/
│   ├── user-service.yaml
│   ├── product-service.yaml
│   ├── order-service.yaml
│   └── gateway-service.yaml
├── screenshots/
│   ├── pods.png               # Screenshot of running pods
│   ├── logs.png               # Screenshot of internal curl tests
│   └── service-test.png       # Screenshot of external API test via port-forward
└── README.md                  # Project documentation and deployment guide
```

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

------

## 🔥 Minikube Setup
```bash
minikube start --driver=docker --memory=4096 --cpus=2
eval $(minikube docker-env)
```

----

## 🐳 Build Docker Images (Inside Minikube Docker)
```bash
docker build -t user-service:latest .
docker build -t product-service:latest .
docker build -t order-service:latest .
docker build -t gateway-service:latest .
```
check image
```bash
docker image
```
---

## ⚙️ Kubernetes Deployment

### Apply Deployments
```bash
kubectl apply -f deployments/
```
### Apply Services
```bash
kubectl apply -f services/
```
---

## ✅ Verify Components

### View All Pods
```bash
kubectl get pods -o wide
```

### View All Services
```bash
kubectl get svc
```

![Screenshot 2025-06-28 170054](https://github.com/user-attachments/assets/78deca1f-8746-4b31-9e89-dd4e70f49fef)<br>

---

## 🔄 Inter-Service Communication (Internal Test)
### Step 1: Enter Gateway Pod
```bash
kubectl get pods -l app=gateway
kubectl exec -it <gateway-pod-name> -- sh
```
### Step 2: Test with Curl
```bash
curl http://user-service:3000/health
curl http://product-service:3001/health
curl http://order-service:3002/health
```

### 📸 Screenshot responses as screenshots/logs.png

![Screenshot 2025-06-28 172046](https://github.com/user-attachments/assets/cd9803f3-c5ab-4cee-af44-906279106f3c)<br>


----

## 🌐 External Testing (Port Forwarding)
### Forward Gateway Service

```bash
kubectl port-forward svc/gateway-service 9090:3003
```
### Test from Browser or Curl

```bash
curl http://localhost:9090/users
curl http://localhost:9090/products
curl http://localhost:9090/orders
```

### 📸 Screenshot results as screenshots/service-test.png

![Screenshot 2025-06-28 171001](https://github.com/user-attachments/assets/a1abaf19-f699-4744-ac8b-50252d190579)<br>

![Screenshot 2025-06-28 171037](https://github.com/user-attachments/assets/59f22c3a-3c3b-4208-af42-8ec30253e849)<br>

----

## 🧪 Health & Debugging
Check Logs
```bash
kubectl logs <pod-name>
```

### Describe Resources
```bash
kubectl describe pod <pod-name>
kubectl describe svc <service-name>
```

----


## 🛠️ Troubleshooting Tips

| Issue                    | Solution                                                    |
| ------------------------ | ----------------------------------------------------------- |
| `ImagePullBackOff`       | Use `imagePullPolicy: Never` and build inside Minikube      |
| Port 8080 already in use | Try `kubectl port-forward svc/gateway-service 9090:3003`    |
| No response on curl      | Use `kubectl exec` into pods and debug with internal `curl` |


-------


## 🧾 Deliverables

-Kubernetes deployment and service YAMLs <br>
-Screenshots folder: <br>
   -pods.png: Pod status <br>
   -logs.png: Internal curl responses <br>
   -service-test.png: External test via port-forward <br>
-This README.md <br>

