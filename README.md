# AWS EKS GitOps DevOps Platform

Production-grade CI/CD and GitOps implementation on AWS EKS using Terraform, GitHub Actions, ArgoCD, and AWS ALB.

This repository serves as the documented platform engineering layer built around a cloud-native microservices application.

---

## ⚠️ Important Clarification

This repository does **not** contain the original microservices application code.

The base application originates from the OpenTelemetry Demo project (via a referenced learning repository).

This repository represents the **DevOps platform engineering layer** built on top of that application, including:

- Infrastructure provisioning (Terraform)
- AWS EKS cluster setup
- GitHub Actions CI pipeline
- ArgoCD GitOps implementation
- AWS ALB + IRSA integration
- Deployment automation and troubleshooting documentation

The working implementation repository where the deployment was executed is:

👉 https://github.com/sunil7chinta/Devops-otel-project

---

## 📌 Project Purpose

This project demonstrates a real-world DevOps workflow including:

- Infrastructure provisioning using Terraform
- Remote state management with S3 and DynamoDB
- Kubernetes cluster deployment on AWS EKS
- GitOps-based continuous deployment using ArgoCD
- CI automation using GitHub Actions
- AWS ALB integration using IRSA (IAM Roles for Service Accounts)

This repository focuses on architecture, automation, and deployment engineering.

---

## 🔗 Repository Relationships

### 🔹 Original Working Implementation (Primary Development Repo)

All actual deployments, CI pipelines, and cluster configuration were executed and validated in:

👉 https://github.com/sunil7chinta/Devops-otel-project

This includes:
- GitHub Actions workflow
- Kubernetes manifests
- Image updates
- ArgoCD sync validation
- Live AWS deployment

---

### 🔹 This Repository (Showcase & Documentation)

👉 https://github.com/sunil7chinta/aws-eks-gitops-devops-platform

This repository provides:

- Clean architectural structure
- Production-grade documentation
- GitOps application manifests
- Infrastructure explanation
- Deployment flow breakdown
- Troubleshooting documentation
- Screenshots as deployment proof

This is designed as a portfolio-grade case study.

---

### 🔹 Reference Repository

Learning reference used during implementation:

https://github.com/iam-veeramalla/ultimate-devops-project-demo

The application source originates from that repository.  
This project builds a production-grade platform layer on top of the application source.

- Terraform provisioning
- AWS EKS setup
- GitHub Actions CI
- ArgoCD GitOps CD
- ALB Controller integration
- IRSA configuration
- Production-style documentation

---

## 🏗️ Architecture Overview

High-Level Deployment Flow:

Developer Push  
→ GitHub Actions (Build + Push Image)  
→ Manifest Image Tag Update  
→ Commit to Git  
→ ArgoCD Detects Change  
→ Auto Sync  
→ Rolling Deployment on EKS  
→ AWS ALB Routes Traffic  

Detailed breakdown available in:

docs/architecture.md

---

## 🧱 Infrastructure (Terraform)

Provisioned resources:

- VPC with public & private subnets
- Amazon EKS cluster
- Managed Node Groups
- IAM roles and policies
- OIDC provider
- S3 backend for Terraform state
- DynamoDB for state locking

### Remote Backend Configuration

Terraform backend configured with:

- S3 for centralized state
- DynamoDB for locking

Ensures:

- State consistency
- Concurrency protection
- Production-level IaC workflow

---

## ☸️ Kubernetes Layer

Deployed components:

- Deployments
- Services
- Ingress (ALB-based)
- ReplicaSets
- Rolling update strategy

Namespaces used:

- `default` → application workloads
- `argocd` → GitOps controller

---

## 🔁 CI/CD Pipeline

### Continuous Integration (GitHub Actions)

Triggered on push to `main`:

1. Build Docker image
2. Tag image with commit SHA
3. Push image to DockerHub
4. Update Kubernetes manifest image tag
5. Commit updated manifest

No manual image updates required.

---

### Continuous Deployment (GitOps via ArgoCD)

ArgoCD monitors the Git repository.

When image tag changes:

- Application marked OutOfSync
- Auto-sync triggered
- Kubernetes rolling update executed
- Old pod terminated
- New pod becomes healthy

No manual kubectl apply after bootstrap.

---

## 🌐 AWS Load Balancer Integration

Configured using:

- AWS Load Balancer Controller
- IAM Role for Service Account (IRSA)
- OIDC provider
- Ingress annotations

Result:

- Dynamic ALB provisioning
- Target groups auto-managed
- Internet-facing access
- Rolling traffic shift during deployment

---

## 📸 Deployment Validation

This platform was fully deployed and validated on AWS.

Due to cloud cost considerations, infrastructure is not continuously running.

Proof available under:

docs/screenshots/

Includes:

- Terraform provisioning logs
- EKS cluster creation
- ALB creation
- ArgoCD sync dashboard
- ReplicaSet revisions
- Rolling deployment evidence
- CI success logs

---

## 🛠️ Key Engineering Challenges Solved

- Terraform state locking configuration
- VPC dependency deletion errors
- EBS disk expansion
- OIDC & IRSA misconfiguration
- ALB controller Helm conflicts
- DNS propagation issues
- ServiceAccount conflicts
- Kubernetes rolling update validation

Full breakdown available in:

docs/troubleshooting.md

---

## 📂 Repository Structure

```text
.
├── terraform/                # Infrastructure as Code (EKS, VPC, IAM, backend)
├── kubernetes/               # Deployment, Service, Ingress manifests
├── argocd/
│   └── applications/         # ArgoCD GitOps application definitions
├── .github/workflows/        # GitHub Actions CI pipeline
├── docs/
│   ├── architecture.md       # High-level architecture explanation
│   ├── deployment-flow.md    # End-to-end CI/CD breakdown
│   ├── troubleshooting.md    # Real-world issues & resolutions
│   └── screenshots/          # Deployment proof (AWS, ArgoCD, CI)
└── README.md
```

---

## 🎯 Skills Demonstrated

- Infrastructure as Code (Terraform)
- GitOps implementation
- AWS EKS provisioning
- ALB + IRSA integration
- CI/CD automation
- Container lifecycle management
- Kubernetes deployment strategies
- Cloud troubleshooting
- Distributed system debugging

---

## 🧠 What Makes This Project Valuable

This is not a simple Kubernetes demo.

It demonstrates:

- End-to-end automated deployment
- Remote state infrastructure management
- Git-driven production workflow
- Real AWS cloud deployment
- IAM security integration
- Zero-downtime rolling updates
- Debugging real cloud issues

This mirrors real-world DevOps and Platform Engineering environments.

---