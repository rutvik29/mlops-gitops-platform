# ⚙️ MLOps GitOps Platform

[![Terraform](https://img.shields.io/badge/Terraform-1.8-7B42BC?style=flat&logo=terraform)](https://terraform.io)
[![Kubernetes](https://img.shields.io/badge/Kubernetes-1.30-326CE5?style=flat&logo=kubernetes)](https://kubernetes.io)
[![ArgoCD](https://img.shields.io/badge/Argo_CD-2.11-EF7B4D?style=flat)](https://argoproj.github.io/cd/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

> **Production MLOps GitOps platform** — Terraform provisions infra, Kubernetes runs models, Argo CD handles GitOps deployments, with canary releases, auto-rollback, and full observability.

## ✨ Highlights

- 🏗️ **Terraform IaC** — EKS/GKE cluster + VPC + RDS + S3 in one apply
- 🔄 **GitOps with Argo CD** — push to git → automatic model deployment
- 🐤 **Canary releases** — 5% → 25% → 100% traffic shifting with metrics gates
- 🔙 **Auto-rollback** — reverts deployment if error rate or latency spikes
- 📊 **Full observability** — Prometheus + Grafana + custom ML metrics
- 🧪 **Model validation** — shadow mode testing before full promotion

## Stack

| Layer | Technology |
|-------|-----------|
| IaC | Terraform 1.8 + Terragrunt |
| Orchestration | Kubernetes 1.30 (EKS) |
| GitOps | Argo CD 2.11 |
| Service Mesh | Istio + Argo Rollouts |
| Monitoring | Prometheus + Grafana + MLflow |
| CI/CD | GitHub Actions |
| Registry | ECR + S3 (model artifacts) |

## Quick Start

```bash
git clone https://github.com/rutvik29/mlops-gitops-platform
cd mlops-gitops-platform

# 1. Provision infrastructure
cd terraform/
terraform init && terraform apply

# 2. Install platform components
./scripts/install_platform.sh

# 3. Deploy a model
kubectl apply -f k8s/model-deployments/sentiment-model.yaml

# 4. Trigger canary release
git tag v1.2.0 && git push --tags
```

## Deployment Pipeline

```
Developer pushes code
        │
GitHub Actions (test + build + push image)
        │
        ▼
Update Argo CD GitOps repo
        │
        ▼
Argo CD detects diff → applies to K8s
        │
        ▼
Argo Rollouts: 5% canary
        │ (metrics OK)
        ▼
25% → 100% promotion
        │ (metrics BAD)
        └──▶ Auto-rollback
```

## License
MIT © Rutvik Trivedi
