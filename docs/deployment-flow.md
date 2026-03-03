# Deployment Flow

## 1. Developer Push

Code changes are pushed to the main branch.

---

## 2. CI Pipeline (GitHub Actions)

Triggered automatically on push.

Pipeline stages:

1. Build Docker image
2. Tag image with commit SHA
3. Push image to DockerHub
4. Update Kubernetes manifest image tag
5. Commit updated manifest back to repository

This ensures the cluster always tracks the Git state.

---

## 3. ArgoCD Sync

ArgoCD continuously monitors the Git repository.

When a new commit is detected:

- Compares desired state vs cluster state
- Marks application as OutOfSync
- Automatically syncs (auto-sync enabled)
- Applies updated manifest

---

## 4. Kubernetes Rolling Deployment

Kubernetes performs rolling update:

- New ReplicaSet created
- New pod scheduled
- Health checks validated
- Old pod terminated gracefully

Zero-downtime deployment achieved.

---

## 5. Traffic Routing

Ingress resource configured with:

- ALB Ingress Class
- Internet-facing scheme

AWS Load Balancer Controller:

- Provisions ALB
- Creates target groups
- Registers nodes
- Routes traffic to healthy pods

---

## 6. Result

End-to-end automated deployment:

Push → Build → Image Update → Git Commit → ArgoCD Sync → Rolling Deployment → Live Traffic

No manual kubectl apply required after initial setup.

---

## GitOps Validation Evidence

Screenshots available in:

docs/screenshots/

Demonstrates:
- Sync OK status
- ReplicaSet revision change
- Pod image update
- Successful rolling update