# 🐍 Flask Kubernetes App

This project demonstrates how to build, containerize, and deploy a simple Flask-based microservice using Kubernetes. It is designed for local development and learning, using Minikube as the Kubernetes environment.

---

## ✨ Features

- 🐳 Docker-based containerization  
- ⚙️ Kubernetes Deployment, Service, and Ingress  
- 🧪 Local testing with Minikube (Docker driver)  
- 🧼 Clean production-style YAML configs  

---

## 🚀 Quick Start

### 1. Start Minikube

```bash
minikube start
minikube addons enable ingress
```

⚠️ **Note:** This guide assumes you are using the Docker driver (the default on macOS and Windows).  
If you're on Linux, Ingress and NodePort access will work more directly.

---

### 2. Build and Load Docker Image into Minikube

```bash
eval $(minikube docker-env)
docker build -t user-service:latest .
```

---

### 3. Apply Kubernetes Resources

```bash
kubectl apply -f k8s/
```

This includes:

- `deployment.yaml`: Runs the Flask app in a pod  
- `service.yaml`: Exposes the app internally via ClusterIP  
- `ingress.yaml`: Routes HTTP requests to the service  

---

## 🌐 Accessing the App

By default, the app exposes a health check at:

```
GET /ping → {"message": "pong"}
```

---

## ⚠️ Local Access Notes (macOS & Windows, Docker Driver)

If you're using Docker on macOS or Windows, Minikube runs in a lightweight VM.  
This causes networking issues where `minikube ip` may not be reachable.

### ✅ Option 1: Use Minikube Service Proxy

```bash
minikube service user-service --url
```

This command launches a local tunnel and prints a usable `http://127.0.0.1:<port>` URL.  
Visit that URL with `/ping` appended.

### ✅ Option 2: Port Forwarding

```bash
kubectl port-forward service/user-service 8080:80
```

Then visit:

```bash
curl http://localhost:8080/ping
```

Both approaches bypass Ingress and allow testing even when direct IP access fails.

---

## 🧠 Architecture Overview

```
Ingress
  ↓
Service (ClusterIP)
  ↓
Deployment → Pod → Flask (Gunicorn) on port 5000
```

---

## 🛠 Project Structure

```
flask-k8s-app/
├── services/
│   └── user-service/
│       ├── app.py
│       ├── Dockerfile
│       ├── requirements.txt
│       └── k8s/
│           ├── deployment.yaml
│           ├── service.yaml
│           └── ingress.yaml
├── README.md
└── .gitignore
```

---

## 🪜 Next Steps

✅ Add a second service (e.g., auth or data API)  
✅ Use Helm to package and template resources  
🔐 Add Kubernetes secrets or ConfigMaps  
📦 Push to GitHub with a public portfolio-ready commit history  

---

## 📚 Additional Resources

- [Minikube Networking Docs](https://minikube.sigs.k8s.io/docs/handbook/accessing/)
- [Kubernetes Ingress](https://kubernetes.io/docs/concepts/services-networking/ingress/)
- [Gunicorn Docs](https://docs.gunicorn.org/en/stable/)

---

## ✨ Author

Built by David Clary as a self-directed project to demonstrate Kubernetes deployment and local testing workflows.
