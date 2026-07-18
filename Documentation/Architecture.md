# Architecture

## Project Title

**Kubernetes-Based Employee Management System on AWS EC2 using Minikube**

---

# Solution Overview

This project demonstrates the deployment of a containerized Flask-based Employee Management System on a Kubernetes cluster running through Minikube inside an AWS EC2 instance.

The application consists of:

- Flask Web Application
- PostgreSQL Database
- Docker Containerization
- Kubernetes Deployments
- Kubernetes Services
- Docker Hub Integration
- AWS EC2 Cloud Infrastructure

---

# High-Level Architecture

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
Docker
    |
    v
Minikube
    |
    v
Kubernetes Cluster
    |
    +----------------------------+
    |                            |
    v                            v
Flask Deployment         PostgreSQL Deployment
    |                            |
    v                            v
Flask Pods (3 Replicas)   PostgreSQL Pod
    |
    v
Employee App Service
    |
    v
Port Forward
    |
    v
User Browser
```

---

# Kubernetes Architecture

```text
                    User
                      |
                      v
          Employee App Service
                      |
                      v
             Flask Deployment
                      |
         -------------------------
         |           |           |
         v           v           v
       Pod 1       Pod 2       Pod 3
         |
         v
          PostgreSQL Service
                   |
                   v
             PostgreSQL Pod
```

---

# Component Description

## AWS EC2

AWS EC2 hosts the complete Kubernetes environment.

Responsibilities:

- Linux Operating System
- Docker Runtime
- Minikube Cluster
- Kubernetes Workloads

---

## Docker

Docker is used to containerize the Employee Management System.

Responsibilities:

- Build application image
- Store application dependencies
- Package source code

Example:

```bash
docker build -t usnr661/employee-management-app:v1 .
```

---

## Docker Hub

Docker Hub stores the application image.

Responsibilities:

- Image Repository
- Version Management
- Kubernetes Image Distribution

Example:

```bash
docker push usnr661/employee-management-app:v1
```

---

## Minikube

Minikube provides a single-node Kubernetes cluster.

Responsibilities:

- Kubernetes Control Plane
- Container Orchestration
- Resource Management

Example:

```bash
minikube start --driver=docker
```

---

## Kubernetes Deployment

Deployments manage application Pods and maintain availability.

### Flask Deployment

Responsibilities:

- Maintain 3 replicas
- Self-healing
- Rolling updates

Configuration:

```yaml
replicas: 3
```

### PostgreSQL Deployment

Responsibilities:

- Run PostgreSQL database
- Manage database lifecycle

Configuration:

```yaml
replicas: 1
```

---

## Kubernetes Pods

Pods are the smallest deployable Kubernetes units.

### Flask Pods

Each Pod contains:

```text
Flask Application Container
```

Current Deployment:

```text
3 Pods
```

Benefits:

- High Availability
- Fault Tolerance
- Load Distribution

---

### PostgreSQL Pod

Contains:

```text
PostgreSQL Database Container
```

Responsibilities:

- Data Storage
- Employee Records
- Database Transactions

---

## Kubernetes Services

Services provide stable networking and service discovery.

### Employee App Service

Purpose:

```text
User → Flask Application
```

Responsibilities:

- Traffic Routing
- Pod Abstraction
- Stable Endpoint

---

### PostgreSQL Service

Purpose:

```text
Flask → PostgreSQL
```

Responsibilities:

- Internal Communication
- Service Discovery

Flask connects using:

```text
postgres-service
```

instead of:

```text
localhost
```

---

# Request Flow

The following diagram illustrates the complete request flow:

```text
User Request
      |
      v
Employee App Service
      |
      v
Flask Pod
      |
      v
PostgreSQL Service
      |
      v
PostgreSQL Pod
      |
      v
Database Response
      |
      v
Flask Application
      |
      v
User Browser
```

---

# Deployment Workflow

The application deployment follows the sequence below:

```text
Source Code
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
Kubernetes Deployment
      |
      v
Pods Created
      |
      v
Services Created
      |
      v
Application Available
```

---

# Kubernetes Features Demonstrated

## Self-Healing

When a Pod fails:

```text
Pod Failure
     |
Deployment Detects Failure
     |
New Pod Created Automatically
```

Benefit:

```text
High Availability
```

---

## Horizontal Scaling

Current:

```text
3 Pods
```

Scaling:

```bash
kubectl scale deployment employee-app --replicas=5
```

Result:

```text
5 Running Pods
```

Benefit:

```text
Improved Performance
Higher Capacity
Better Availability
```

---

## Service Discovery

Kubernetes automatically provides internal DNS.

Example:

```text
postgres-service
```

Benefits:

- No hardcoded IP addresses
- Dynamic networking
- Easy service communication

---

# Final Architecture Summary

The project successfully demonstrates:

✅ AWS EC2 Cloud Deployment

✅ Docker Containerization

✅ Docker Hub Integration

✅ Minikube Kubernetes Cluster

✅ Flask Deployment

✅ PostgreSQL Deployment

✅ Kubernetes Services

✅ Kubernetes Secrets

✅ Service Discovery

✅ Self-Healing

✅ Horizontal Scaling

✅ Container Orchestration

✅ Real-World Application Deployment

The Employee Management System was successfully deployed on Kubernetes using Minikube running in AWS EC2, providing hands-on experience with modern cloud-native application deployment and administration.