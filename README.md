# Full Stack Chat Application - Kubernetes Deployment

## Overview

This project is a Full Stack Chat Application originally forked from:

https://github.com/iemafzalhassan

The objective of this work was to understand containerization and Kubernetes orchestration by deploying the application locally on a Minikube cluster.

---

## What I Modified

I did not develop the application from scratch. Instead, I forked the original repository and customized the Kubernetes deployment configuration to make the application run successfully on my local machine using Minikube.

The major changes were focused on the `k8s` directory.

---

## Kubernetes Components Created/Modified

### Namespace

Created a dedicated namespace:

```yaml
chat-app
```

to isolate all application resources.

### MongoDB

Configured:

* MongoDB Deployment
* MongoDB Service
* Persistent Volume Claim (PVC)

Configured MongoDB credentials:

```text
Username: root
Password: admin/password
```

and ensured persistent storage for database data.

### Backend

Configured:

* Backend Deployment
* Backend Service

Added required environment variables:

```text
NODE_ENV
PORT
MONGODB_URI
JWT_SECRET
```

Configured the backend to connect to MongoDB using Kubernetes Service Discovery:

```text
mongodb://root:admin@mongodb:27017/chatApp?authSource=admin
```

### Frontend

Configured:

* Frontend Deployment
* Frontend Service

Ensured the frontend could communicate with the backend through Kubernetes networking.

---

## Issues Faced and Resolved

### 1. Docker Image Push Failure

Encountered:

```text
push access denied
repository does not exist
```

Resolved by:

* Logging into Docker Hub
* Using the correct Docker Hub username
* Tagging images correctly

---

### 2. Persistent Volume Claim Not Found

Encountered:

```text
persistentvolumeclaim "mongodb-pvc" not found
```

Resolved by:

* Creating the PVC
* Applying PVC before MongoDB deployment

---

### 3. Invalid Kubernetes YAML Configuration

Encountered deployment validation errors caused by:

* Incorrect indentation
* Invalid fields
* Incorrect port definitions

Resolved by correcting Deployment specifications.

---

### 4. Frontend CrashLoopBackOff

Encountered:

```text
host not found in upstream "backend"
```

Resolved by:

* Creating the backend service
* Correcting service names inside Nginx configuration

---

### 5. Backend CrashLoopBackOff

Encountered:

```text
MONGODB_URI environment variable is required
```

Resolved by adding the required environment variables to the backend deployment.

---

### 6. MongoDB Connectivity Issues

Encountered:

```text
getaddrinfo EAI_AGAIN mongodb-service
```

Resolved by:

* Creating MongoDB service
* Using the correct Kubernetes service name:

```text
mongodb
```

instead of:

```text
mongodb-service
```

---

### 7. Service Port Mismatch

Backend container:

```text
5000
```

Backend service:

```text
5001
```

Resolved by configuring:

```yaml
port: 5001
targetPort: 5000
```

---

### 8. ImagePullBackOff Errors

Encountered:

```text
pull access denied
repository does not exist
```

Resolved by:

* Updating deployment files to use correct Docker images
* Replacing invalid image references
* Using official MongoDB image:

```text
mongo:latest
```

---

## Final Architecture

```text
Frontend (NodePort)
        |
        v
Backend Service (NodePort)
        |
        v
MongoDB Service (ClusterIP)
        |
        v
MongoDB Pod + Persistent Storage
```

---

## Kubernetes Resources

### Deployments

* frontend
* backend
* mongodb

### Services

* frontend (NodePort)
* backend (NodePort)
* mongodb (ClusterIP)

### Storage

* Persistent Volume Claim (PVC)

### Namespace

* chat-app

---

## Verification

Verified successful deployment using:

```bash
kubectl get all -n chat-app
```

Result:

* Frontend Running
* Backend Running
* MongoDB Running
* All Deployments Available

Backend logs confirmed:

```text
server is running on PORT:5001
MongoDB connected: mongodb
```

---

## Learning Outcomes

Through this project I gained practical experience with:

* Docker image management
* Docker Hub image publishing
* Kubernetes Deployments
* Kubernetes Services
* Kubernetes Namespaces
* Persistent Volumes and PVCs
* Environment Variables and Secrets
* Service Discovery
* Pod troubleshooting
* CrashLoopBackOff debugging
* ImagePullBackOff debugging
* Local Kubernetes deployment using Minikube

---

## Author

Shaik Shafeeq

Based on the original project by:
https://github.com/iemafzalhassan
