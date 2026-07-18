# Troubleshooting & Issues Faced

This section documents the major technical issues encountered during the deployment of the Kubernetes-Based Employee Management System and the solutions used to resolve them.

---

## 1. Docker Image Pull Failure (ImagePullBackOff)

### Error

Pods remained in the following state:

```text
ImagePullBackOff
```

### Cause

The Flask deployment referenced:

```yaml
image: usnr661/employee-management-app:latest
```

However, the Docker Hub repository only contained:

```text
usnr661/employee-management-app:v1
```

### Solution

Updated the deployment manifest:

```yaml
image: usnr661/employee-management-app:v1
```

Applied the changes:

```bash
kubectl apply -f flask-deployment.yaml
kubectl rollout restart deployment employee-app
```

### Result

```text
All Flask application pods started successfully.
```

---

## 2. Insufficient EC2 Memory

### Error

```text
RSRC_INSUFFICIENT_CONTAINER_MEMORY

docker only has 908MiB available
```

### Cause

The original EC2 instance did not provide enough memory to run:

- Docker
- Minikube
- Kubernetes Control Plane
- PostgreSQL
- Flask Application Pods

### Verification

```bash
free -h
```

Output:

```text
Mem: 908Mi
```

### Solution

Upgraded the EC2 instance type to provide approximately:

```text
8 GB RAM
```

### Result

```text
Minikube started successfully and Kubernetes became stable.
```

---

## 3. Insufficient Storage Space

### Error

```text
no space left on device
```

### Cause

Docker images, Kubernetes components, and Minikube consumed most of the available storage.

### Verification

```bash
df -h
```

Minikube also displayed warnings related to low disk space.

### Solution

Increased the AWS EBS volume size to:

```text
100 GB
```

Removed unused Docker resources:

```bash
docker system prune -a
```

### Result

```text
Additional storage became available and deployments completed successfully.
```

---

## 4. PostgreSQL Table Not Found

### Error

```text
psycopg2.errors.UndefinedTable:

relation "employees" does not exist
```

### Cause

A fresh PostgreSQL deployment was created inside Kubernetes and the required application table had not yet been created.

### Solution

Connected to PostgreSQL and created the table manually:

```sql
CREATE TABLE employees (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100),
    email VARCHAR(100),
    department VARCHAR(100)
);
```

### Result

```text
The Flask application successfully retrieved employee data.
```

---

## 5. NodePort Access Failure

### Problem

The application was accessible using:

```bash
curl http://$(minikube ip):30080
```

but not through:

```text
http://EC2_PUBLIC_IP:30080
```

### Investigation

Verified:

```bash
kubectl get pods
```

```bash
kubectl get svc
```

```bash
kubectl get endpoints employee-app-service
```

The outputs confirmed:

```text
Pods Running ✅
Services Healthy ✅
Endpoints Available ✅
Database Connected ✅
Application Running ✅
```

### Root Cause

Minikube was running with the Docker driver:

```bash
minikube start --driver=docker
```

In this configuration, NodePort traffic was available inside the Minikube network but not directly exposed through the EC2 public interface.

### Solution

Used Kubernetes port forwarding:

```bash
kubectl port-forward service/employee-app-service 5000:5000 --address 0.0.0.0
```

Accessed the application through:

```text
http://EC2_PUBLIC_IP:5000
```

### Result

```text
The website became accessible from the browser.
```

---

## 6. Minikube Profile Error

### Error

```text
Profile "minikube" not found
```

while running:

```bash
sudo minikube tunnel
```

### Cause

The active Kubernetes environment did not contain the expected Minikube profile configuration.

### Solution

Verified cluster status:

```bash
kubectl get nodes
kubectl cluster-info
```

Confirmed the cluster remained functional and continued using the existing deployment.

### Result

```text
Project functionality was not affected.
```

---

## 7. Learning Outcomes from Troubleshooting

Throughout this project, multiple real-world infrastructure and deployment issues were encountered and resolved.

Skills gained include:

- Docker troubleshooting
- Kubernetes debugging
- Service discovery
- Pod management
- PostgreSQL administration
- AWS EC2 resource management
- Storage management
- Kubernetes networking
- Container orchestration
- Infrastructure troubleshooting
- Linux administration

The troubleshooting process significantly improved practical DevOps and Kubernetes administration skills.