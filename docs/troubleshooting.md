# Troubleshooting Log

This section documents real issues encountered during deployment and how they were resolved.

---

## 1. Terraform VPC Deletion Error

Issue:
DependencyViolation when deleting VPC.

Cause:
Dependent resources (subnets, ENIs, Load Balancers) still attached.

Resolution:
Destroyed dependent resources before deleting VPC.

---

## 2. EC2 Disk Full

Issue:
"No space left on device"

Cause:
Default EBS volume size insufficient.

Resolution:
- Increased EBS volume size
- Resized filesystem using growpart + resize2fs

---

## 3. ALB Controller Installation Failure

Issue:
Helm install failed — name already in use

Cause:
Existing service account and IAM stack conflict.

Resolution:
- Deleted existing IAM service account via eksctl
- Deleted associated CloudFormation stack
- Recreated IAM service account with correct policy
- Reinstalled AWS Load Balancer Controller

---

## 4. ALB Not Accessible via DNS

Issue:
DNS resolution failure

Cause:
Propagation delay / incorrect host mapping

Resolution:
Validated Ingress address
Verified ALB creation
Confirmed target group health
Used correct ALB DNS name

---

## 5. ArgoCD Application OutOfSync

Issue:
Application temporarily OutOfSync during image update

Expected Behavior:
Occurs when Git change detected before sync completes

Resolution:
Auto-sync enabled → resolved automatically

---

## 6. OIDC and IRSA Misconfiguration

Issue:
IAM Role not attaching properly to Service Account

Cause:
Incorrect annotation or policy ARN

Resolution:
Verified:
- OIDC provider exists
- Correct policy ARN attached
- Service account annotation correct

---

## Lessons Learned

- Always configure Terraform remote backend
- Validate IRSA carefully
- ALB controller requires precise IAM permissions
- Rolling deployments can temporarily show terminating pods (normal behavior)
- GitOps reduces manual intervention significantly