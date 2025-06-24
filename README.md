# 🗳️ 3-Tier Microservice Voting App with ArgoCD & Azure DevOps  
**Production-grade deployment leveraging GitOps and CI/CD best practices**  

![Architecture Diagram]
C:\Downloads\68747470733a2f2f6d69726f2e6d656469756d2e636f6d2f76322f726573697a653a6669743a3730302f312a314e5947784c6175785642494f4d5a4c716e4e7755412e706e67.png
)
) *(Example architecture diagram - replace with your actual diagram)*  

---

## 📋 Table of Contents  
- [🌐 Architecture Overview](#-architecture-overview)
- [⚙️ Prerequisites](#️-prerequisites)
- [🐳 Local Deployment](#-local-deployment-with-docker-compose)
- [🔄 CI/CD Pipeline](#-cicd-pipeline-setup)
- [☸️ Kubernetes Deployment](#️-kubernetes-deployment)
- [🔍 Monitoring](#-monitoring--troubleshooting)
- [📸 Screenshots](#-screenshots)
- [❓ FAQs](#-faqs)

---

## 🌐 Architecture Overview

```mermaid
graph TD
    A[User] -->|Votes| B(vote:8080)
    B -->|Stores| C[(Redis:6379)]
    C -->|Processed by| D(worker)
    D -->|Persists to| E[(Postgres:5432)]
    E -->|Read by| F(result:5000)
    A -->|Views Results| F
Component Matrix:

Service	Tech Stack	Port	Purpose
vote	Python/Flask	8080	User voting interface
result	Node.js/Express	5000	Real-time results dashboard
worker	.NET Core	-	Vote processor (Redis→Postgres)
redis	Redis	6379	Temporary vote storage
postgres	PostgreSQL	5432	Persistent vote storage
Infrastructure Flow:

CI/CD: Azure DevOps → ACR

GitOps: ArgoCD syncs AKS with repo changes

Monitoring: Prometheus + Grafana (optional)

⚙️ Prerequisites
🛠️ Tools
bash
# Required Tools
az --version        # Azure CLI 2.30+
kubectl version     # Kubernetes 1.20+
docker --version    # Docker 20.10+
argocd version      # ArgoCD 2.3+
☁️ Azure Resources
Resource	Recommended Spec
AKS Cluster	2 nodes, Standard_D2s_v3
ACR	Standard Tier
Service Principal	Contributor permissions
🐳 Local Deployment
bash
# 1. Clone repository
git clone https://github.com/Akshatkashyap786/voting_app.git
cd voting_app

# 2. Start all services
docker-compose up -d

# 3. Verify services
docker-compose ps
Access Interfaces:

Voting UI: http://localhost:8080

Results: http://localhost:5000

🔄 CI/CD Pipeline Setup
Azure DevOps Configuration
yaml
# Example pipeline snippet (vote-service.yml)
trigger:
  branches:
    include: [ main ]
  paths:
    include: [ vote/* ]

variables:
  dockerRegistry: 'myacr.azurecr.io'
  tag: $(Build.BuildId)

steps:
- task: Docker@2
  inputs:
    command: buildAndPush
    repository: vote
    dockerfile: vote/Dockerfile
    tags: $(tag)
Pipeline Stages:

Build: Containerize each microservice

Push: Store in ACR with versioned tags

Deploy: ArgoCD auto-syncs (GitOps)

☸️ Kubernetes Deployment
AKS Cluster Setup
bash
az aks create \
  --name voting-cluster \
  --node-count 2 \
  --enable-cluster-autoscaler \
  --min-count 1 \
  --max-count 3 \
  --attach-acr myacr
ArgoCD Installation
bash
# 1. Install ArgoCD
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml

# 2. Access UI (Port-forward)
kubectl port-forward svc/argocd-server -n argocd 8081:443
Default Credentials:

Username: admin

Password: kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d

🔍 Monitoring & Troubleshooting
Common Issues
Symptom	Debug Command	Likely Fix
Pod CrashLoop	kubectl logs -f <pod> -c <container>	Check ENV variables
ArgoCD Out-of-Sync	argocd app diff voting-app	Validate manifests in repo
Pipeline Timeout	journalctl -u vsts-agent	Scale up agent VM
