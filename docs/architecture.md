# Architecture Overview

## Project Summary

This project demonstrates a production-style DevOps platform deployed on AWS EKS using:

- Terraform for Infrastructure as Code
- Amazon EKS for container orchestration
- AWS ALB Ingress Controller (IRSA enabled)
- ArgoCD for GitOps-based CD
- GitHub Actions for CI
- Docker for containerization

The infrastructure was fully deployed and validated on AWS. Due to cloud cost considerations, the environment is not continuously running.

---

## High-Level Architecture

The following diagram illustrates the complete CI/CD and GitOps workflow implemented in this project:

![AWS EKS GitOps DevOps Platform Architecture](docs/screenshots/architecture-diagram.png)

> End-to-end DevOps platform architecture integrating Terraform, GitHub Actions, ArgoCD, Amazon EKS, and AWS ALB.

---

### Deployment Flow (Step-by-Step)

GitHub (Source Code)  
 ↓  
GitHub Actions (CI Pipeline)  
 ↓  
Docker Image pushed to DockerHub  
 ↓  
Kubernetes Manifests Updated (Image Tag)  
 ↓  
ArgoCD (GitOps Controller)  
 ↓  
Amazon EKS Cluster  
 ↓  
AWS Application Load Balancer  
 ↓  
Public Access via DNS

---

## Infrastructure Layer (Terraform)

Provisioned using Terraform with remote backend configuration:

- S3 bucket for state storage
- DynamoDB table for state locking
- VPC with public and private subnets
- EKS cluster
- Node groups
- IAM roles and policies

Remote backend ensures:

- State consistency
- Concurrency protection
- Production-grade infrastructure management

---

## Kubernetes Layer

- Deployments
- Services
- Ingress (ALB-based)
- Rolling updates enabled
- ReplicaSets managed automatically

Workloads are deployed to the `default` namespace.
ArgoCD runs in the `argocd` namespace.

---

## GitOps Layer (ArgoCD)

ArgoCD was installed using official manifests.

Features enabled:

- Auto Sync
- Prune
- Self Heal

When a new image tag is committed:

- ArgoCD detects Git change
- Syncs automatically
- Triggers rolling deployment
- Old ReplicaSet scales down
- New ReplicaSet becomes active

---

## Load Balancing

AWS Load Balancer Controller configured using:

- OIDC provider
- IAM Role for Service Account (IRSA)
- Annotated Ingress resources

Result:

- ALB dynamically provisioned
- Target groups auto-managed
- Internet-facing access enabled

---

## Observability

OpenTelemetry demo application used to simulate a microservices architecture.
Pod health and sync state validated via ArgoCD dashboard.

---

## Key Engineering Concepts Demonstrated

- Infrastructure as Code
- GitOps
- Immutable container deployments
- IRSA authentication model
- Remote Terraform state management
- CI/CD automation
- Cloud-native ingress architecture
