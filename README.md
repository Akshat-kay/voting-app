# 🚀 Deploy a 3-Tier Microservice Voting App using ArgoCD and Azure DevOps Pipeline

This project demonstrates a complete CI/CD pipeline for a Dockerized microservices-based voting application using Azure DevOps, AKS (Azure Kubernetes Service), Azure Container Registry (ACR), and GitOps with ArgoCD.

---

## 🧾 Overview

The Voting App consists of six microservices:

- **vote (Frontend)** – Python/Flask web app to cast votes
- **vote processor** – Node.js backend service
- **Redis** – Stores votes temporarily
- **worker** – Python service that moves votes from Redis to PostgreSQL
- **result** – Flask web app displaying vote results
- **PostgreSQL** – Final vote store

---

## 🛠️ Tech Stack

- Azure DevOps (CI/CD)
- Azure Kubernetes Service (AKS)
- Azure Container Registry (ACR)
- Docker & Docker Compose
- ArgoCD (GitOps)
- Kubernetes
- Redis, PostgreSQL
- Python (Flask), Node.js

---

## 🧱 Architecture

![Architecture](https://raw.githubusercontent.com/Akshat-kay/voting-app/main/architecture.excalidraw.png)

---

## 📦 Project Stages

### 🔹 Stage 1: Continuous Integration (CI)

1. **Clone and run app locally using Docker Compose**
2. **Create Azure DevOps project and import Git repo**
3. **Create Azure Container Registry (ACR)**
4. **Configure self-hosted agent on Azure VM**
5. **Write Azure DevOps pipelines for each microservice (`vote`, `result`, `worker`)**
   - Build and push Docker images to ACR

![2-Stage Pipeline](https://raw.githubusercontent.com/Akshat-kay/voting-app/main/2stage_pipe.png)
![Azure DevOps Pipeline](https://raw.githubusercontent.com/Akshat-kay/voting-app/main/axure_pipe.png)
![ACR Images](https://raw.githubusercontent.com/Akshat-kay/voting-app/main/acr-images.png)

---

### 🔹 Stage 2: Continuous Delivery (CD)

1. **Create Azure Kubernetes Service (AKS) cluster**
2. **Install Azure CLI and `kubectl`**
3. **Install ArgoCD in the AKS cluster**
4. **Expose ArgoCD via NodePort and access UI**
5. **Connect ArgoCD to Azure DevOps Git repo**
6. **Deploy microservices using `k8s-specifications/` manifests**
7. **Enable automatic sync for GitOps delivery**

![ArgoCD UI](https://raw.githubusercontent.com/Akshat-kay/voting-app/main/argocd-ui.png)
![ArgoCD Apps View](https://raw.githubusercontent.com/Akshat-kay/voting-app/main/argocdui2.png)
![ArgoCD GitHub Integration](https://raw.githubusercontent.com/Akshat-kay/voting-app/main/argocdXgithubrepo.png)
![Kubernetes Pods](https://raw.githubusercontent.com/Akshat-kay/voting-app/main/argocdpods.png)

---

## 🧪 How to Run Locally

```bash
# SSH into Azure VM
ssh -i <your-key.pem> azureuser@<vm-public-ip>

# Install Docker & Docker Compose
sudo apt update
sudo apt install docker.io docker-compose -y

# Clone and start app
git clone https://github.com/Akshat-kay/voting-app.git
cd voting-app
docker-compose up -d
