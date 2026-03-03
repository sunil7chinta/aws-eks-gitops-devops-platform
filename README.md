# AWS EKS GitOps DevOps Platform

Production-grade CI/CD and GitOps implementation on AWS EKS using Terraform, GitHub Actions, ArgoCD, and AWS Application Load Balancer (ALB).

This repository documents the **DevOps and platform engineering layer** built around a cloud-native microservices application.

---

## 📊 Project Status

- Fully implemented and validated on AWS EKS
- CI/CD workflow executed successfully
- ArgoCD auto-sync verified
- Infrastructure currently not running due to cloud cost considerations

---

## 📌 Project Scope & Clarification

This repository does **not** contain the original microservices business logic.

The base application originates from the OpenTelemetry Demo project (via a referenced learning repository).

What this repository represents is the **complete cloud infrastructure, CI/CD automation, and GitOps deployment architecture** engineered and implemented by me on top of that application.

---

## 🔗 Repository Relationships

### 🔹 Primary Implementation Repository (Execution & Deployment)

All AWS deployments, CI execution, ArgoCD sync validation, and infrastructure testing were performed in:

👉 https://github.com/sunil7chinta/Devops-otel-project

This includes:
- Terraform execution
- GitHub Actions pipeline runs
- Kubernetes deployment validation
- ArgoCD live sync
- AWS ALB provisioning
- Production-style debugging

---

### 🔹 This Repository (Platform Engineering Showcase)

👉 https://github.com/sunil7chinta/aws-eks-gitops-devops-platform

This repository provides:

- Structured infrastructure code
- Declarative GitOps manifests
- Architecture documentation
- Deployment flow breakdown
- Troubleshooting logs
- Deployment proof screenshots
- Clean portfolio presentation

It is designed as a **platform engineering case study**, not an application repository.

---

### 🔹 Application Base Reference

Application source referenced during implementation:

https://github.com/iam-veeramalla/ultimate-devops-project-demo

This project implements a production-style DevOps platform architecture on top of that application.

---

## 🎯 Scope of Engineering Contribution

### ❌ What Was Not Built Here

- Microservices business logic
- Internal service implementation
- Application feature development

---

### ✅ What Was Engineered in This Project

The following components were engineered and implemented as part of this project:

### Infrastructure (Terraform)
- VPC with public and private subnets
- Amazon EKS cluster
- Managed node groups
- IAM roles and policies
- OIDC provider setup
- S3 backend for Terraform state
- DynamoDB state locking

### Kubernetes & Networking
- Deployment and Service manifests
- ALB-based Ingress configuration
- Rolling deployment strategy
- Namespace separation
- ReplicaSet lifecycle validation

### GitHub Actions CI Pipeline
- Automated Docker build and image tagging
- Registry push (DockerHub)
- Kubernetes manifest update
- Git commit triggering GitOps deployment

### GitOps (ArgoCD)
- Declarative Application manifest
- Auto-sync enabled
- Self-healing enabled
- Prune enabled
- Rolling updates triggered via Git commit

### AWS Integration
- AWS Load Balancer Controller
- IRSA (IAM Role for Service Account)
- Target group automation
- Internet-facing ALB routing

### Real-World Debugging
- Terraform state locking issues
- VPC dependency deletion errors
- EBS disk expansion
- IAM & OIDC misconfiguration
- ALB controller conflicts
- DNS propagation troubleshooting
- ServiceAccount recreation errors

---

## 🔍 Responsibility Breakdown

| Component | Application Repo | This Platform Repo |
|------------|-----------------|-------------------|
| Microservices Code | ✔ | ✖ |
| Business Logic | ✔ | ✖ |
| Dockerization | Partial | ✔ |
| Terraform Infrastructure | ✖ | ✔ |
| AWS EKS Provisioning | ✖ | ✔ |
| GitHub Actions CI (productcatalog-service) | ✔ | ✖ |
| ArgoCD GitOps | ✖ | ✔ |
| ALB + IRSA Setup | ✖ | ✔ |
| Deployment Automation | ✖ | ✔ |
| Troubleshooting Documentation | ✖ | ✔ |

---

## 🏗️ Architecture Overview

High-Level Deployment Flow:

Developer Push  
→ GitHub Actions (Build & Push Image)  
→ GitHub Actions updates Kubernetes manifest (image tag)  
→ Updated manifest committed to repository   
→ ArgoCD Detects Change  
→ Auto Sync  
→ Rolling Deployment on EKS  
→ AWS ALB Routes Traffic  

Detailed architecture breakdown available in:

docs/architecture.md

---

## 🧱 Infrastructure (Terraform)

Provisioned Resources:

- VPC with public/private subnets
- Amazon EKS cluster
- Managed node groups
- IAM roles and policies
- OIDC provider
- S3 backend for state storage
- DynamoDB for state locking

### Remote Backend Configuration

Terraform configured with:

- S3 (remote state storage)
- DynamoDB (state locking)

Ensures:
- Concurrency-safe infrastructure changes
- Centralized state management
- Production-style IaC workflow

---

## ☸️ Kubernetes Layer

Deployed Components:

- Deployments
- Services
- Ingress (ALB-based)
- ReplicaSets
- Rolling update strategy

Namespaces:

- `default` → application workloads
- `argocd` → GitOps controller

---

## 🔄 CI/CD Design Approach

The CI workflow is triggered on push to the `main` branch.

The CI pipeline is currently implemented for the **productcatalog-service** as a reference microservice.

Pipeline stages include:

1. Docker image build
2. Image tagging using commit SHA
3. Push to DockerHub
4. GitHub Actions updates Kubernetes manifest (image tag)
5. Commit pushed to repository

This design can be extended to additional microservices using reusable workflow patterns.

---

### Continuous Deployment (GitOps via ArgoCD)

ArgoCD monitors the repository.

On manifest update:

- Application marked OutOfSync
- Auto-sync applies changes
- Kubernetes performs rolling update
- Old pod terminates
- New pod becomes active

No manual kubectl apply required after bootstrap.

---

## 🌐 AWS Load Balancer Integration

Configured using:

- AWS Load Balancer Controller
- IRSA (IAM Role for Service Account)
- OIDC provider
- Ingress annotations

Result:

- Dynamic ALB provisioning
- Automatic target group registration
- Internet-facing routing
- Zero-downtime deployment behavior

---

## 📸 Deployment Validation

The platform was fully deployed and validated on AWS.

Due to cloud cost considerations, infrastructure is not continuously running.

Deployment proof available in:

docs/screenshots/

Includes:

- Terraform provisioning logs
- EKS cluster creation
- ALB provisioning
- ArgoCD sync dashboard
- ReplicaSet revisions
- Rolling update validation
- CI pipeline execution logs

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

## 🛠️ Key Engineering Concepts Demonstrated

- Infrastructure as Code
- Remote Terraform state management
- GitOps workflow implementation
- CI/CD automation
- IAM & IRSA integration
- Kubernetes networking
- Rolling deployments
- Cloud debugging & troubleshooting
- Production-style deployment architecture

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

## 📈 Project Metrics

- 30+ AWS resources provisioned via Terraform
- 3 containerized microservices deployed
- 1 end-to-end automated CI pipeline implemented
- GitOps auto-sync enabled with zero-downtime rolling updates
- Remote state locking configured using DynamoDB

---

## 🚀 Future Enhancements

Planned improvements:

- Prometheus and Grafana monitoring integration
- Horizontal Pod Autoscaler (HPA)
- Cluster Autoscaler
- HTTPS with AWS ACM
- Image vulnerability scanning (Trivy)
- Multi-environment (dev/staging/prod) deployment strategy

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
