# Learning Journey

## Project Overview

This project began as a learning exercise to understand Kubernetes fundamentals and evolved into a complete deployment of a real-world Flask-based Employee Management System running on Kubernetes in AWS EC2.

Instead of deploying a simple sample application, I chose to deploy an existing project that already included:

- Flask Application
- PostgreSQL Database
- Docker Containerization
- AWS Deployment Experience

The objective was to understand how Kubernetes manages containerized applications through Pods, Deployments, Services, and networking.

---

# Objectives

The main goals of this project were:

- Learn Kubernetes fundamentals
- Deploy a real application using Kubernetes
- Understand Deployments and Services
- Manage PostgreSQL within Kubernetes
- Learn Service Discovery
- Understand Pod scaling and self-healing
- Practice troubleshooting Kubernetes environments
- Gain hands-on cloud experience using AWS EC2

---

# Technologies Used

## Cloud

- AWS EC2

## Containerization

- Docker
- Docker Hub

## Container Orchestration

- Kubernetes
- Minikube
- kubectl

## Application Stack

- Python
- Flask
- PostgreSQL

---

# What I Learned

## Docker Image Lifecycle

I learned how applications are packaged into Docker images and stored inside Docker Hub.

Workflow:

```text
Application Source Code
        |
Docker Build
        |
Docker Image
        |
Docker Hub
        |
Kubernetes Pulls Image
```

Key Commands:

```bash
docker build
docker push
docker pull
```

---

## Kubernetes Architecture

Before this project, Kubernetes concepts were mostly theoretical.

During this project, I learned the relationship between:

```text
Deployment
    |
ReplicaSet
    |
Pods
```

and how Kubernetes continuously maintains the desired application state.

---

## Pods

I learned that Pods are the smallest deployable units in Kubernetes.

Example:

```text
Employee Application Pod
PostgreSQL Pod
```

I also learned how to inspect Pods:

```bash
kubectl get pods
kubectl describe pod
kubectl logs
```

---

## Deployments

Deployments simplified application management.

Instead of manually starting containers, Kubernetes automatically managed Pods for me.

Example:

```yaml
replicas: 3
```

This created:

```text
Pod 1
Pod 2
Pod 3
```

and ensured they remained running.

---

## Self-Healing

One of the most interesting Kubernetes features was self-healing.

After deleting a running Pod:

```bash
kubectl delete pod <pod-name>
```

Kubernetes automatically created a replacement Pod.

This demonstrated how Deployments maintain the desired state of the application.

---

## Scaling Applications

I learned how Kubernetes scales applications horizontally.

Example:

```bash
kubectl scale deployment employee-app --replicas=5
```

Result:

```text
5 Running Pods
```

This allowed the application to serve more traffic without modifying the application code.

---

## Kubernetes Services

Services provide stable networking for applications.

Without Services:

```text
Pods receive dynamic IP addresses.
```

With Services:

```text
Applications communicate using service names.
```

Example:

```text
postgres-service
```

instead of:

```text
localhost
```

This concept was essential for communication between Flask and PostgreSQL.

---

## Secrets Management

I learned how Kubernetes stores sensitive information securely.

Example:

```yaml
kind: Secret
```

Used to store:

- Database Username
- Database Password
- Database Name

This approach is safer than hardcoding credentials into application code.

---

## AWS and Kubernetes

This project provided practical experience deploying Kubernetes on AWS EC2.

I learned:

- Instance resource planning
- Security Group configuration
- SSH administration
- Storage management
- Resource monitoring

---

# Challenges Faced

Several real-world issues were encountered during deployment.

## ImagePullBackOff

Cause:

```text
Incorrect Docker image tag.
```

Solution:

```text
Updated deployment to use the correct image version.
```

---

## Insufficient Memory

Cause:

```text
The EC2 instance had insufficient resources.
```

Solution:

```text
Upgraded instance to provide approximately 8 GB RAM.
```

---

## Insufficient Storage

Cause:

```text
Docker and Kubernetes consumed available disk space.
```

Solution:

```text
Expanded EBS volume to 100 GB.
```

---

## Missing PostgreSQL Table

Cause:

```text
Fresh PostgreSQL deployment created an empty database.
```

Solution:

```text
Created the employees table manually.
```

---

## NodePort Networking Issue

Cause:

```text
Minikube running with Docker driver on AWS EC2.
```

Solution:

```bash
kubectl port-forward service/employee-app-service 5000:5000 --address 0.0.0.0
```

This exposed the application successfully.

---

# Skills Developed

Through this project I gained hands-on experience with:

- Kubernetes Administration
- Docker Containerization
- AWS EC2 Management
- Linux Administration
- Service Discovery
- Kubernetes Networking
- Pod Management
- Deployment Management
- PostgreSQL Administration
- Cloud Infrastructure
- Troubleshooting and Debugging
- Container Orchestration

---

# Key Takeaways

The most important lesson from this project was that Kubernetes is much more than simply running containers.

Kubernetes provides:

- Automated Deployment
- Self-Healing
- High Availability
- Service Discovery
- Horizontal Scaling
- Centralized Management

I also learned the importance of systematic troubleshooting by analyzing logs, deployments, services, endpoints, networking, and resource utilization.

---

# Final Outcome

Successfully deployed a Dockerized Employee Management System using Kubernetes running on Minikube inside AWS EC2.

Final architecture included:

```text
Flask Application
        |
Kubernetes Service
        |
Flask Pods (3 Replicas)
        |
PostgreSQL Service
        |
PostgreSQL Pod
```

The project successfully demonstrated:

✅ Docker

✅ Kubernetes

✅ Minikube

✅ AWS EC2

✅ PostgreSQL

✅ Pod Management

✅ Deployments

✅ Services

✅ Scaling

✅ Self-Healing

✅ Cloud Administration

✅ Real-World Troubleshooting

This project significantly improved my practical understanding of Kubernetes and cloud-native application deployment.