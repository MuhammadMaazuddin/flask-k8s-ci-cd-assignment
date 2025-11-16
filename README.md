---

````md
# Flask K8s CI/CD Assignment

This project demonstrates a complete **CI/CD pipeline** for deploying a Flask application to **Kubernetes** using **Docker**, **Jenkins**, and **K8s deployment strategies** such as automated rollouts, scaling, and load balancing.

---

## 🚀 Project Overview

This repository contains a simple **Flask REST API** containerized using Docker and deployed on a Kubernetes cluster.  
The CI/CD pipeline is implemented using **Jenkins**, with automated build, test, and deploy stages.

### ✅ Kubernetes Features Used

- **Deployment** – Manages replicas and ensures zero-downtime rollouts.
- **Service (ClusterIP / NodePort)** – Provides stable networking for pods.
- **ReplicaSets** – Ensures the desired number of pod replicas are running.
- **Rolling Updates** – Allows smooth deployment without downtime.
- **Autoscaling (HPA Optional)** – Scales pods automatically based on CPU load.
- **Load Balancing** – K8s service distributes traffic across pods.

---

## 🐳 Run the Application Locally Using Docker

### 1️⃣ Make sure you have Docker installed  
<!-- Check version: -->
```bash
docker --version
````

### 2️⃣ Build the Docker image

```bash
docker build -t flask-app:latest .
```

### 3️⃣ Run the Docker container

```bash
docker run -p 5000:5000 flask-app:latest
```

### 4️⃣ Test locally

Open in browser:

```
http://localhost:5000
```

---

## ☸️ Deploy to Kubernetes Using Jenkins Pipeline

This repo includes a Jenkinsfile with the following stages:

### **Pipeline Stages**

#### **1. Build Docker Image**

Uses Minikube’s Docker daemon:

```bash
eval $(minikube docker-env)
docker build -t flask-k8s:latest .
```

#### **2. Apply Kubernetes Manifests**

Jenkins runs:

```bash
kubectl apply -f k8s/deployment.yaml
kubectl apply -f k8s/service.yaml
```

#### **3. Verify Deployment**

Jenkins checks rollout:

```bash
kubectl rollout status deployment/flask-app
```

#### **4. Expose Service**

To access the app:

```bash
minikube service flask-app-service
```

---

## ⚙️ Automated Rollouts, Scaling, and Load Balancing

### 🔄 **Automated Rollouts**

Kubernetes Deployments automatically:

* Upgrade pods without downtime
* Roll back when a bad image is pushed
* Maintain availability during update

You can trigger rollout manually:

```bash
kubectl rollout restart deployment flask-app
```

Rollback:

```bash
kubectl rollout undo deployment flask-app
```

---

### 📈 **Scaling**

You can manually scale the app:

```bash
kubectl scale deployment flask-app --replicas=3
```

Or enable autoscaling (optional):

```bash
kubectl autoscale deployment flask-app \
  --cpu-percent=70 --min=1 --max=5
```

---

### ⚖️ **Load Balancing**

Kubernetes Services automatically:

* Distribute traffic across replicas
* Ensure no single pod becomes overloaded

Service example:

```yaml
type: NodePort
selector:
  app: flask-app
ports:
  - port: 5000
    targetPort: 5000
```

---

## 📁 Repository Structure

```
.
├── app.py
├── Dockerfile
├── Jenkinsfile
├── k8s/
│   ├── deployment.yaml
│   └── service.yaml
└── README.md
```

---

## 🧩 Workflow Diagram

                ┌────────────────────┐
                │ Developer Pushes Code
                └────────────┬───────┘
                             │
                ┌────────────▼────────────┐
                │ GitHub Actions CI Triggered 
                └────────────┬────────────┘
                             │
      ┌──────────────────────▼────────────────────────┐
      │ 1️⃣ Checkout → 2️⃣ Setup Python → 3️⃣ Install deps 
      └──────────────────────┬────────────────────────┘
                             │
                ┌────────────▼────────────┐
                │ 4️⃣ flake8 Linting       │
                └────────────┬────────────┘
                             │
                ┌────────────▼────────────┐
                │ 5️⃣ pytest Unit Tests    │
                └────────────┬────────────┘
                             │
                ┌────────────▼────────────┐
                │ 6️⃣ Docker Build         │
                └────────────┬────────────┘
                             │
                    ✔️ CI Pass / ❌ Fail


## 👤 Author

**Muhammad Maazuddin Qureshi**
**Sammar Kaleem**
Flask, Kubernetes, Jenkins, CI/CD

---


