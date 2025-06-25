
- **Frontend (vote)**: Flask app for casting votes.
- **Backend (vote processor)**: Node.js microservice.
- **Redis**: Temporary vote queue.
- **Worker**: Python service to move votes from Redis to PostgreSQL.
- **Results**: Flask app displaying the results.
- **PostgreSQL**: Final data store for votes.

---

## 📦 Project Stages

---

### 🛠️ Stage 1: Continuous Integration (CI)

#### 1. Clone & Run Locally
- Created an Azure Ubuntu VM
- Installed Docker and Docker Compose
- Cloned repo and ran `docker-compose up -d`

#### 2. Create Azure DevOps Project
- Imported GitHub repo into Azure DevOps Repos

#### 3. Create Azure Container Registry
- Created ACR instance for storing container images

#### 4. Set Up Self-Hosted Agent
- Configured Azure DevOps to use the same Ubuntu VM
- Installed agent using `config.sh` and `run.sh`

#### 5. Create CI Pipelines
- Wrote individual pipeline scripts for:
  - `vote`
  - `worker`
  - `result`
- Used separate build and push stages to ACR

---

### 🚢 Stage 2: Continuous Delivery (CD)

#### 1. Create Azure Kubernetes Service (AKS)
- Configured AKS with autoscaling node pool

#### 2. Install CLI Tools
- Installed Azure CLI and `kubectl` on a new VM
- Connected to AKS using `az aks get-credentials`

#### 3. Install ArgoCD
- Installed ArgoCD in AKS namespace using bash script
- Exposed ArgoCD via NodePort for public access

#### 4. Configure ArgoCD
- Connected ArgoCD to Azure Repo via HTTPS + Personal Access Token (PAT)
- Synced Kubernetes manifests from `k8s-specifications/`
- Enabled auto-sync and self-healing deployments

---

## 📷 Screenshots

> Upload these images in your GitHub repo or LinkedIn post

- Docker containers running locally
- Azure DevOps pipeline success
- Images pushed to Azure Container Registry
- Kubernetes pods running via `kubectl`
- ArgoCD UI showing sync status
- ArgoCD application deployment success

---

## 🧪 How to Run Locally

```bash
# Create an Azure Ubuntu VM and SSH into it
ssh -i <key.pem> azureuser@<public-ip>

# Install Docker and Docker Compose
sudo apt update && sudo apt install docker.io docker-compose -y

# Clone the repo
git clone https://github.com/yourusername/voting-app-devops.git
cd voting-app-devops

# Spin up the services
docker-compose up -d

# Visit http://<public-ip>:8080 in browser
