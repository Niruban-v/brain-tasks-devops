# Brain Tasks App - DevOps Deployment

## Project Overview

This project demonstrates end-to-end deployment of a React application using Docker, AWS ECR, Amazon EKS, Kubernetes, AWS CodeBuild, and AWS CodePipeline.

The application was containerized, pushed to AWS Elastic Container Registry (ECR), and deployed into an Amazon EKS Kubernetes cluster using Kubernetes deployment and service manifests.

---

# Technologies Used

* React Application
* Docker
* AWS ECR
* AWS EKS
* Kubernetes
* AWS EC2
* AWS CodeBuild
* AWS CodePipeline
* CloudWatch Logs
* GitHub

---

# Application Repository

Original Repository:

https://github.com/Vennilavanguvi/Brain-Tasks-App.git

---

# Dockerization

## Build Docker Image

```bash
docker build -t brain-tasks-app .
```

## Run Docker Container

```bash
docker run -d -p 3000:80 brain-tasks-app
```

---

# AWS ECR

## Create ECR Repository

```bash
aws ecr create-repository --repository-name brain-tasks-app --region ap-south-1
```

## Login to ECR

```bash
aws ecr get-login-password --region ap-south-1 | docker login --username AWS --password-stdin 678419967121.dkr.ecr.ap-south-1.amazonaws.com
```

## Tag Docker Image

```bash
 docker tag brain-tasks-app:latest 678419967121.dkr.ecr.ap-south-1.amazonaws.com/brain-tasks-app:latest
```

## Push Docker Image

```bash
docker push 678419967121.dkr.ecr.ap-south-1.amazonaws.com/brain-tasks-app:latest
```

---

# Kubernetes Deployment

## deployment.yaml

Creates Kubernetes deployment for Brain Tasks application.

## service.yaml

Creates Kubernetes LoadBalancer service to expose the application publicly.

---

# Amazon EKS Setup

## Create EKS Cluster

```bash
eksctl create cluster \
--name brain-tasks-cluster \
--region ap-south-1 \
--nodegroup-name workers \
--node-type t3.small \
--nodes 1 \
--managed
```

## Verify Nodes

```bash
kubectl get nodes
```

---

# Deploy Application to Kubernetes

```bash
kubectl apply -f deployment.yaml
kubectl apply -f service.yaml
```

## Verify Services

```bash
kubectl get svc
```

---

# Application LoadBalancer URL

http://a1c60bbee10464848987ddaa9354169f-2139949820.ap-south-1.elb.amazonaws.com

---

# AWS CodeBuild

CodeBuild was configured to:

* Pull source from GitHub
* Build Docker image
* Push image to AWS ECR
* Deploy Kubernetes manifests to EKS

The build process is defined in:

```bash
buildspec.yml
```

---

# AWS CodePipeline

Pipeline Flow:

GitHub → CodeBuild → ECR → EKS Deployment

Stages:

1. Source
2. Build
3. Deploy

---

# CloudWatch Monitoring

CloudWatch Logs were used to monitor:

* Build logs
* Deployment logs
* Kubernetes application logs

---

# Submission Items

* GitHub Repository
* Dockerfile
* deployment.yaml
* service.yaml
* buildspec.yml
* README.md
* Screenshots
* LoadBalancer URL

---

# Cleanup

To avoid AWS charges:

```bash
eksctl delete cluster --name brain-tasks-cluster --region ap-south-1
```

Delete:

* EKS Cluster
* EC2 Instance
* ECR Repository
* LoadBalancer

---
