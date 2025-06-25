# 🚀 Deploy a 3-Tier Microservice Voting App using ArgoCD and Azure DevOps Pipeline

This project demonstrates a complete DevOps CI/CD pipeline for a microservices-based Voting App using Azure DevOps, Docker, Kubernetes (AKS), Azure Container Registry (ACR), and ArgoCD.

---

## 🧾 Overview

The application is composed of 6 microservices:
- **vote (Frontend)** – Python/Flask app where users cast votes
- **vote processor** – Node.js backend that receives votes
- **Redis** – Temporarily stores votes
- **worker** – Python service that processes votes from Redis to PostgreSQL
- **result** – Flask app that displays voting results
- **PostgreSQL** – Stores the final vote count

All components are containerized with Docker and deployed using Kubernetes. CI/CD is managed via Azure DevOps and ArgoCD with GitOps principles.

---

## 🛠️ Technologies Used

- Azure DevOps (CI/CD Pipelines)
- Docker & Docker Compose
- Azure Container Registry (ACR)
- Azure Kubernetes Service (AKS)
- ArgoCD (GitOps)
- Kubernetes
- Python, Node.js
- Redis & PostgreSQL

---

## 📦 Project Workflow

### 🔹 Stage 1: Continuous Integration (CI)
1. **Local Deployment**:
   - Deployed the app on an Azure Ubuntu VM using Docker Compose.

2. **Azure DevOps Setup**:
   - Created project and imported GitHub repo.
   - Built CI pipelines for `vote`, `worker`, and `result` services.

3. **ACR Integration**:
   - Docker images are built and pushed to Azure Container Registry.

4. **Self-Hosted Agent**:
   - Azure DevOps pipelines are run on a custom VM-based agent.

<p align="center">
  <img src="screenshots/2stage_pipe.png" alt="2 Stage CI Pipeline" width="700"/>
  <br>
  <em>2-stage CI pipeline for image build and push</em>
</p>

<p align="center">
  <img src="screenshots/acr-images.png" alt="ACR Images" width="700"/>
  <br>
  <em>Images stored in Azure Container Registry</em>
</p>

---

### 🔹 Stage 2: Continuous Delivery (CD)
1. **AKS Cluster**:
   - Created and configured an Azure Kubernetes Service cluster.

2. **ArgoCD Installation**:
   - Deployed ArgoCD into AKS.
   - Exposed ArgoCD using NodePort and logged in using decoded credentials.

3. **GitOps Deployment**:
   - ArgoCD continuously syncs Kubernetes manifests from Azure Repos.

<p align="center">
  <img src="screenshots/argocd-ui.png" alt="ArgoCD UI" width="700"/>
  <br>
  <em>ArgoCD dashboard view</em>
</p>

<p align="center">
  <img src="screenshots/argocdui2.png" alt="ArgoCD Applications" width="700"/>
  <br>
  <em>Application successfully deployed using ArgoCD</em>
</p>

<p align="center">
  <img src="screenshots/argocdpods.png" alt="Kubernetes Pods" width="700"/>
  <br>
  <em>All microservices running as pods on AKS</em>
</p>

<p align="center">
  <img src="screenshots/argocdXgithubrepo.png" alt="GitHub Repo Connected" width="700"/>
  <br>
  <em>ArgoCD connected to GitHub repo</em>
</p>

---

## 🗺️ Architecture Diagram

<p align="center">
  <img src="screenshots/architecture.excalidraw.png" alt="Architecture Diagram" width="800"/>
  <br>
  <em>Full CI/CD + GitOps Architecture</em>
</p>

---

## 🧪 How to Run Locally

```bash
# SSH into your Azure VM
ssh -i <your-key.pem> azureuser@<vm-public-ip>

# Install Docker & Docker Compose
sudo apt update
sudo apt install docker.io docker-compose -y

# Clone the repo and run the app
git clone https://github.com/yourusername/voting-app-devops.git
cd voting-app-devops
docker-compose up -d
