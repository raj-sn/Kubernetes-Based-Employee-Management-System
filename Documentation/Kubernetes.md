# Kubernetes Concepts

This document explains the core Kubernetes concepts used in the deployment of the Employee Management System.

---

# What is Kubernetes?

Kubernetes is an open-source container orchestration platform used to deploy, manage, scale, and automate containerized applications.

Instead of manually managing containers, Kubernetes provides automated management of application workloads through Deployments, Pods, Services, and other resources.

---

# Project Architecture

```text
User
  |
  v
Employee App Service
  |
  v
Flask Deployment
  |
  +---------+---------+
  |         |         |
  v         v         v
Pod 1     Pod 2     Pod 3
  |
  v
PostgreSQL Service
  |
  v
PostgreSQL Pod

Hosted On:

AWS EC2
  |
Docker
  |
Minikube
  |
Kubernetes Cluster
```

---

# Kubernetes Components Used

## Pod

A Pod is the smallest deployable unit in Kubernetes.

A Pod contains one or more containers and provides the environment where containers run.

### Example

```text
PostgreSQL Pod
```

Contains:

```text
PostgreSQL Container
```

And:

```text
Employee Application Pod
```

Contains:

```text
Flask Container
```

### Commands

View Pods:

```bash
kubectl get pods
```

Describe Pod:

```bash
kubectl describe pod <pod-name>
```

View Logs:

```bash
kubectl logs <pod-name>
```

---

## Deployment

A Deployment manages Pods and ensures the desired number of replicas remain running.

### Example

```yaml
replicas: 3
```

Kubernetes automatically creates:

```text
Employee App Pod 1
Employee App Pod 2
Employee App Pod 3
```

If a Pod fails:

```text
Pod Deleted
      |
Deployment Detects Failure
      |
New Pod Created
```

This feature is called:

```text
Self-Healing
```

### Commands

View Deployments:

```bash
kubectl get deployments
```

Describe Deployment:

```bash
kubectl describe deployment employee-app
```

---

## Service

A Service provides stable network communication between applications.

Pods frequently receive new IP addresses when recreated.

Instead of connecting directly to a Pod IP, applications communicate using Services.

### Example

Flask connects to PostgreSQL through:

```text
postgres-service
```

instead of:

```text
localhost
```

### Service Flow

```text
Flask Pods
     |
postgres-service
     |
PostgreSQL Pod
```

### Commands

View Services:

```bash
kubectl get svc
```

Describe Service:

```bash
kubectl describe svc employee-app-service
```

---

## Secret

Secrets securely store sensitive information such as database credentials.

### Stored Information

```text
POSTGRES_USER
POSTGRES_PASSWORD
POSTGRES_DB
```

### Example

```yaml
kind: Secret
```

Benefits:

```text
Improved Security
Centralized Credential Management
Avoids Hardcoding Credentials
```

### Commands

View Secrets:

```bash
kubectl get secrets
```

---

## ReplicaSet

A ReplicaSet ensures the desired number of Pod replicas are running at all times.

ReplicaSets are automatically created and managed by Deployments.

### Relationship

```text
Deployment
     |
ReplicaSet
     |
Pods
```

### Commands

View ReplicaSets:

```bash
kubectl get rs
```

---

# Kubernetes Features Demonstrated

## Self-Healing

Kubernetes automatically replaces failed Pods.

### Example

Delete a Pod:

```bash
kubectl delete pod <pod-name>
```

Result:

```text
Old Pod Deleted
     |
Deployment Detects Change
     |
New Pod Created
```

### Benefit

```text
Improved Availability
Reduced Downtime
Automatic Recovery
```

---

## Horizontal Scaling

Kubernetes allows applications to scale horizontally by increasing the number of replicas.

### Example

Scale Deployment:

```bash
kubectl scale deployment employee-app --replicas=5
```

Result:

```text
5 Running Pods
```

### Benefit

```text
Higher Availability
Improved Performance
Increased Capacity
```

---

## Service Discovery

Kubernetes automatically creates internal DNS records for Services.

Applications communicate using Service names.

### Example

PostgreSQL service:

```text
postgres-service
```

Flask automatically resolves:

```text
postgres-service
```

to the PostgreSQL Pod.

### Benefit

```text
No Manual IP Management
Reliable Communication
Automatic DNS Resolution
```

---

# Deployment Workflow

The Employee Management System deployment follows the workflow below:

```text
Source Code
      |
Docker Build
      |
Docker Image
      |
Docker Hub
      |
AWS EC2
      |
Minikube
      |
Kubernetes Cluster
      |
      +-------------------+
      |                   |
      v                   v
Flask Deployment   PostgreSQL Deployment
      |                   |
      v                   v
Flask Pods        PostgreSQL Pod
      |
      v
Employee App Service
      |
      v
User Access
```

---

# Kubernetes Commands Used

## Cluster Information

```bash
kubectl get nodes
```

---

## View Pods

```bash
kubectl get pods
```

---

## View Deployments

```bash
kubectl get deployments
```

---

## View Services

```bash
kubectl get svc
```

---

## View All Resources

```bash
kubectl get all
```

---

## Pod Logs

```bash
kubectl logs <pod-name>
```

---

## Apply Configuration

```bash
kubectl apply -f deployment.yaml
```

---

## Delete Pod

```bash
kubectl delete pod <pod-name>
```

---

## Scale Deployment

```bash
kubectl scale deployment employee-app --replicas=5
```

---

# Summary

During this project, the following Kubernetes concepts were implemented and tested successfully:

✅ Pods

✅ Deployments

✅ ReplicaSets

✅ Services

✅ Secrets

✅ Service Discovery

✅ Self-Healing

✅ Horizontal Scaling

✅ Container Orchestration

✅ Kubernetes Networking

✅ Application Deployment on AWS EC2 using Minikube

The project provided practical experience with real-world Kubernetes administration and deployment of a containerized Flask application connected to a PostgreSQL database.