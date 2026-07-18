# Deployment Guide

This document describes the complete deployment process for the Kubernetes-Based Employee Management System running on AWS EC2 using Docker, Minikube, and Kubernetes.

---

# Prerequisites

Before deployment, ensure the following tools are installed:

### Docker

Verify installation:

```bash
docker --version
```

### kubectl

Verify installation:

```bash
kubectl version --client
```

### Minikube

Verify installation:

```bash
minikube version
```

---

# Deployment Architecture

```text
Developer
    |
    v
Employee Management System Source Code
    |
    v
Docker Build
    |
    v
Docker Image
    |
    v
Docker Hub
    |
    v
AWS EC2
    |
    v
Minikube Kubernetes Cluster
    |
    +----------------------+
    |                      |
    v                      v
Flask Deployment    PostgreSQL Deployment
    |                      |
    v                      v
Flask Pods         PostgreSQL Pod
    |
    v
Employee App Service
    |
    v
User Access
```

---

# Step 1: Build Docker Image

Navigate to the project directory:

```bash
cd Kubernetes-Based-Employee-Management-System
```

Build the Docker image:

```bash
docker build -t usnr661/employee-management-app:v1 .
```

Verify:

```bash
docker images
```

Expected:

```text
usnr661/employee-management-app   v1
```

---

# Step 2: Push Image to Docker Hub

Login to Docker Hub:

```bash
docker login
```

Push image:

```bash
docker push usnr661/employee-management-app:v1
```

Purpose:

```text
Store application image in Docker Hub
so Kubernetes can pull it during deployment.
```

Verify image availability:

```bash
docker pull usnr661/employee-management-app:v1
```

---

# Step 3: Start Kubernetes Cluster

Start Minikube using Docker driver:

```bash
minikube start --driver=docker
```

Verify cluster:

```bash
kubectl get nodes
```

Expected:

```text
NAME       STATUS
minikube   Ready
```

---

# Step 4: Deploy PostgreSQL Secret

Apply PostgreSQL credentials:

```bash
kubectl apply -f k8s/postgres-secret.yaml
```

Verify:

```bash
kubectl get secrets
```

Expected:

```text
postgres-secret
```

---

# Step 5: Deploy PostgreSQL

Create PostgreSQL Deployment:

```bash
kubectl apply -f k8s/postgres-deployment.yaml
```

Verify:

```bash
kubectl get pods
```

Expected:

```text
postgres-xxxxx   Running
```

---

# Step 6: Create PostgreSQL Service

Apply:

```bash
kubectl apply -f k8s/postgres-service.yaml
```

Verify:

```bash
kubectl get svc
```

Expected:

```text
postgres-service
```

Purpose:

```text
Allows Flask Pods to communicate
with PostgreSQL using service discovery.
```

---

# Step 7: Deploy Flask Application

Create Flask Deployment:

```bash
kubectl apply -f k8s/flask-deployment.yaml
```

Verify:

```bash
kubectl get pods
```

Expected:

```text
employee-app-xxxxx   Running
employee-app-yyyyy   Running
employee-app-zzzzz   Running
```

Deployment configuration:

```text
Replicas: 3
Image: usnr661/employee-management-app:v1
```

Purpose:

```text
Deploy Employee Management System
and maintain multiple replicas.
```

---

# Step 8: Create Flask Service

Apply:

```bash
kubectl apply -f k8s/flask-service.yaml
```

Verify:

```bash
kubectl get svc
```

Expected:

```text
employee-app-service
```

Purpose:

```text
Expose Flask Pods internally and
provide stable networking access.
```

---

# Step 9: Verify Deployment

View all Kubernetes resources:

```bash
kubectl get all
```

Expected resources:

```text
Pods
Services
Deployments
ReplicaSets
```

Verify deployments:

```bash
kubectl get deployments
```

Verify services:

```bash
kubectl get svc
```

Verify pods:

```bash
kubectl get pods
```

---

# Step 10: Verify Application Connectivity

Test application through Minikube:

```bash
curl http://$(minikube ip):30080
```

Expected:

```html
Employee Management System
```

This confirms:

```text
Flask Application ✅
PostgreSQL ✅
Kubernetes Networking ✅
```

---

# Step 11: External Access

Due to Minikube Docker driver networking limitations on AWS EC2, NodePort was not directly accessible through the EC2 public interface.

Application was exposed using:

```bash
kubectl port-forward service/employee-app-service 5000:5000 --address 0.0.0.0
```

Access URL:

```text
http://EC2_PUBLIC_IP:5000
```

Result:

```text
Application successfully accessible from browser.
```

---

# Self-Healing Demonstration

Delete a running Pod:

```bash
kubectl delete pod <pod-name>
```

Verify:

```bash
kubectl get pods
```

Result:

```text
Kubernetes automatically creates a replacement pod.
```

This demonstrates:

```text
Self-Healing Capability
```

---

# Scaling Demonstration

Increase replicas:

```bash
kubectl scale deployment employee-app --replicas=5
```

Verify:

```bash
kubectl get pods
```

Result:

```text
5 Running Pods
```

Restore original state:

```bash
kubectl scale deployment employee-app --replicas=3
```

This demonstrates:

```text
Horizontal Scaling
```

---

# Deployment Summary

The Employee Management System was successfully:

✅ Containerized using Docker

✅ Stored in Docker Hub

✅ Deployed to Kubernetes using Minikube

✅ Hosted on AWS EC2

✅ Connected to PostgreSQL

✅ Exposed through Kubernetes Services

✅ Tested for Self-Healing

✅ Tested for Horizontal Scaling

✅ Validated through browser access

The deployment demonstrates core Kubernetes administration, container orchestration, service discovery, scaling, and troubleshooting skills.