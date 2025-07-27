# 🚀 MERN Stack DevSecOps Project - Complete Deployment Guide

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![AWS](https://img.shields.io/badge/AWS-Cloud-orange)](https://aws.amazon.com/)
[![Kubernetes](https://img.shields.io/badge/Kubernetes-Container%20Orchestration-blue)](https://kubernetes.io/)
[![Jenkins](https://img.shields.io/badge/Jenkins-CI%2FCD-red)](https://jenkins.io/)

## 📋 Table of Contents
- [🎯 Project Overview](#-project-overview)
- [🏗️ Architecture](#️-architecture)
- [🛠️ Prerequisites](#️-prerequisites)
- [⚡ Quick Start](#-quick-start)
- [📦 Detailed Setup](#-detailed-setup)
- [🔧 Configuration](#-configuration)
- [🚀 Deployment](#-deployment)
- [📊 Monitoring](#-monitoring)
- [🔒 Security](#-security)
- [🐛 Troubleshooting](#-troubleshooting)
- [🤝 Contributing](#-contributing)

## 🎯 Project Overview

This project demonstrates a **production-ready MERN stack application** with a complete **DevSecOps pipeline** deployed on **AWS using Kubernetes**. It includes automated CI/CD, security scanning, monitoring, and GitOps practices.

### 🌟 Key Features
- **MERN Stack**: MongoDB, Express.js, React.js, Node.js
- **DevSecOps Pipeline**: Jenkins with security integration
- **Container Orchestration**: Kubernetes with AWS EKS
- **Infrastructure as Code**: Terraform for AWS provisioning
- **Security Scanning**: SonarQube, Trivy, OWASP
- **GitOps**: ArgoCD for automated deployments
- **Monitoring**: Prometheus & Grafana
- **Cloud Native**: AWS ALB, ECR, EKS

### 🎯 Application
A simple **To-Do List application** that allows users to:
- ✅ Add new tasks
- ✏️ Edit existing tasks
- ❌ Delete tasks
- ☑️ Mark tasks as complete/incomplete

## 🏗️ Architecture

### 🔧 Technology Stack

| Component | Technology | Purpose |
|-----------|------------|---------|
| **Frontend** | React.js + Material-UI | User Interface |
| **Backend** | Node.js + Express.js | REST API |
| **Database** | MongoDB | Data Storage |
| **CI/CD** | Jenkins (Master-Slave) | Automation Pipeline |
| **Security** | SonarQube, Trivy, OWASP | Code & Container Security |
| **Container** | Docker | Application Packaging |
| **Orchestration** | Kubernetes (EKS) | Container Management |
| **GitOps** | ArgoCD | Deployment Automation |
| **Monitoring** | Prometheus + Grafana | Observability |
| **Infrastructure** | Terraform | Infrastructure as Code |
| **Cloud** | AWS (EC2, VPC, ALB, ECR) | Cloud Platform |

### 🌐 Network Architecture
```
Internet → AWS ALB → Kubernetes Ingress → Services → Pods
                                      ↓
                              Frontend (React)
                              Backend (Express)
                              Database (MongoDB)
```

### 🔄 CI/CD Flow
```
Developer → Git Push → Jenkins → Security Scans → Build → ECR → ArgoCD → Kubernetes
```

## 🛠️ Prerequisites

### 💻 Local Requirements
- **Operating System**: Linux/macOS/Windows with WSL2
- **RAM**: Minimum 8GB (16GB recommended)
- **Storage**: 50GB free space
- **Internet**: Stable connection for downloads

### 🔑 Accounts & Access
1. **AWS Account** with administrative access
2. **GitHub Account** for code repository
3. **Domain Name** (optional, for custom domain)

### 📦 Required Tools
Install these tools on your local machine:

```bash
# Git
sudo apt update && sudo apt install git -y

# AWS CLI v2
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip && sudo ./aws/install

# Terraform
wget -O- https://apt.releases.hashicorp.com/gpg | sudo gpg --dearmor -o /usr/share/keyrings/hashicorp-archive-keyring.gpg
echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/hashicorp.list
sudo apt update && sudo apt install terraform -y

# kubectl
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl

# Helm
curl https://baltocdn.com/helm/signing.asc | gpg --dearmor | sudo tee /usr/share/keyrings/helm.gpg > /dev/null
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/helm.gpg] https://baltocdn.com/helm/stable/debian/ all main" | sudo tee /etc/apt/sources.list.d/helm-stable-debian.list
sudo apt update && sudo apt install helm -y
```

### 🔐 AWS Setup
1. **Create AWS Account** if you don't have one
2. **Create IAM User** with programmatic access
3. **Attach Policies**:
   - `AmazonEC2FullAccess`
   - `AmazonVPCFullAccess`
   - `AmazonEKSClusterPolicy`
   - `AmazonEKSWorkerNodePolicy`
   - `AmazonEKS_CNI_Policy`
   - `AmazonEC2ContainerRegistryFullAccess`
   - `ElasticLoadBalancingFullAccess`

4. **Configure AWS CLI**:
```bash
aws configure
# Enter your Access Key ID
# Enter your Secret Access Key
# Default region: us-east-1 (or your preferred region)
# Default output format: json
```

5. **Create EC2 Key Pair**:
```bash
aws ec2 create-key-pair --key-name devsecops-key --query 'KeyMaterial' --output text > devsecops-key.pem
chmod 400 devsecops-key.pem
```

## ⚡ Quick Start

### 🚀 One-Command Deployment
```bash
# Clone the repository
git clone https://github.com/your-username/MERN-DevSecOps-Project.git
cd MERN-DevSecOps-Project

# Deploy infrastructure
cd Terraform
terraform init
terraform plan -var="key_name=devsecops-key"
terraform apply -var="key_name=devsecops-key" -auto-approve

# Get outputs
terraform output
```

**⏱️ Estimated Time**: 15-20 minutes for complete infrastructure setup

### 🎯 What Gets Deployed
- ✅ AWS VPC with public subnet
- ✅ Jenkins Master server (t3a.medium)
- ✅ Jenkins Agent server (t3a.large)
- ✅ Security groups with proper rules
- ✅ All required software pre-installed

## 📦 Detailed Setup

### Step 1: Infrastructure Deployment

1. **Clone Repository**:
```bash
git clone https://github.com/your-username/MERN-DevSecOps-Project.git
cd MERN-DevSecOps-Project
```

2. **Review Terraform Variables**:
```bash
cd Terraform
cat variables.tf
```

3. **Customize Variables** (optional):
```bash
# Edit terraform.tfvars
cat > terraform.tfvars << EOF
key_name = "your-key-name"
ami = "ami-0c02fb55956c7d316"  # Amazon Linux 2
your_ip = "YOUR_PUBLIC_IP/32"   # Get from: curl ifconfig.me
EOF
```

4. **Deploy Infrastructure**:
```bash
terraform init
terraform validate
terraform plan
terraform apply -auto-approve
```

5. **Save Outputs**:
```bash
terraform output > ../infrastructure-outputs.txt
```

### Step 2: Access Jenkins Master

1. **Get Jenkins Master IP**:
```bash
JENKINS_IP=$(terraform output -raw jenkins_master_public_ip)
echo "Jenkins Master IP: $JENKINS_IP"
```

2. **SSH to Jenkins Master**:
```bash
ssh -i ../devsecops-key.pem ec2-user@$JENKINS_IP
```

3. **Get Jenkins Initial Password**:
```bash
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

4. **Access Jenkins Web UI**:
```
http://JENKINS_IP:8080
```

### Step 3: Jenkins Configuration

1. **Install Suggested Plugins**
2. **Create Admin User**
3. **Install Additional Plugins**:
   - Docker Pipeline
   - Kubernetes
   - SonarQube Scanner
   - OWASP Dependency-Check
   - AWS Steps

4. **Configure Tools** (Manage Jenkins → Global Tool Configuration):
   - **JDK**: Add JDK 11
   - **NodeJS**: Add Node.js 16+
   - **SonarQube Scanner**: Add scanner
   - **Docker**: Add Docker

### Step 4: SonarQube Setup

1. **Access SonarQube**:
```
http://JENKINS_IP:9000
Default: admin/admin
```

2. **Create Project**:
   - Project Key: `three-tier-backend`
   - Display Name: `Three Tier Backend`

3. **Generate Token**:
   - User → My Account → Security → Generate Token

4. **Configure in Jenkins**:
   - Manage Jenkins → Configure System → SonarQube servers
   - Add SonarQube server with token

### Step 5: AWS ECR Setup

1. **Create ECR Repositories**:
```bash
aws ecr create-repository --repository-name three-tier-frontend --region us-east-1
aws ecr create-repository --repository-name three-tier-backend --region us-east-1
```

2. **Add AWS Credentials to Jenkins**:
   - Manage Jenkins → Manage Credentials
   - Add AWS Access Key ID and Secret

### Step 6: Agent Node Setup

1. **Get Agent Node IP**:
```bash
AGENT_IP=$(terraform output -raw agent_node_public_ip)
echo "Agent Node IP: $AGENT_IP"
```

2. **SSH to Agent Node**:
```bash
ssh -i ../devsecops-key.pem ec2-user@$AGENT_IP
```

3. **Verify Installations**:
```bash
docker --version
kubectl version --client
kind version
```

### Step 7: Kubernetes Cluster Setup

1. **On Agent Node, Create kind Cluster**:
```bash
# Create cluster
kind create cluster --config=/home/ec2-user/kind-config.yaml --name three-tier-cluster

# Verify cluster
kubectl cluster-info
kubectl get nodes
```

2. **Install AWS Load Balancer Controller**:
```bash
# Install cert-manager
kubectl apply --validate=false -f https://github.com/jetstack/cert-manager/releases/download/v1.5.4/cert-manager.yaml

# Install AWS Load Balancer Controller
curl -o iam_policy.json https://raw.githubusercontent.com/kubernetes-sigs/aws-load-balancer-controller/v2.4.7/docs/install/iam_policy.json
aws iam create-policy --policy-name AWSLoadBalancerControllerIAMPolicy --policy-document file://iam_policy.json

# Apply controller
kubectl apply -k "github.com/aws/eks-charts/stable/aws-load-balancer-controller//crds?ref=master"
```

### Step 8: ArgoCD Setup

1. **Install ArgoCD**:
```bash
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

2. **Access ArgoCD**:
```bash
# Get initial password
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d

# Port forward
kubectl port-forward svc/argocd-server -n argocd 8080:443
```

3. **Access ArgoCD UI**:
```
https://localhost:8080
Username: admin
Password: [from above command]
```

## 🔧 Configuration

### Jenkins Pipeline Configuration

1. **Create Pipeline Jobs**:
   - New Item → Pipeline → "three-tier-frontend"
   - New Item → Pipeline → "three-tier-backend"

2. **Configure Pipeline**:
   - Pipeline script from SCM
   - Git repository URL
   - Credentials: GitHub token
   - Script Path: `Jenkins-Pipeline-Code/Jenkinsfile-Frontend`

### Environment Variables

Add these credentials in Jenkins:
```
ACCOUNT_ID: Your AWS Account ID
ECR_REPO1: three-tier-frontend
ECR_REPO2: three-tier-backend
GITHUB: GitHub credentials
```

### Kubernetes Secrets

1. **Create Namespace**:
```bash
kubectl create namespace three-tier
```

2. **Create MongoDB Secret**:
```bash
kubectl create secret generic mongo-sec \
  --from-literal=username=admin \
  --from-literal=password=password123 \
  -n three-tier
```

## 🚀 Deployment

### Manual Deployment

1. **Deploy Database**:
```bash
kubectl apply -f Kubernetes-Manifests-file/Database/ -n three-tier
```

2. **Deploy Backend**:
```bash
kubectl apply -f Kubernetes-Manifests-file/Backend/ -n three-tier
```

3. **Deploy Frontend**:
```bash
kubectl apply -f Kubernetes-Manifests-file/Frontend/ -n three-tier
```

4. **Deploy Ingress**:
```bash
kubectl apply -f Kubernetes-Manifests-file/ingress.yaml -n three-tier
```

### Automated Deployment via Jenkins

1. **Trigger Frontend Pipeline**:
   - Go to Jenkins → three-tier-frontend → Build Now

2. **Trigger Backend Pipeline**:
   - Go to Jenkins → three-tier-backend → Build Now

3. **Monitor Deployment**:
```bash
kubectl get pods -n three-tier -w
kubectl get svc -n three-tier
kubectl get ingress -n three-tier
```

### Verify Deployment

1. **Check Pod Status**:
```bash
kubectl get pods -n three-tier
```

2. **Check Services**:
```bash
kubectl get svc -n three-tier
```

3. **Get Application URL**:
```bash
kubectl get ingress -n three-tier
```

## 📊 Monitoring

### Prometheus & Grafana Setup

1. **Add Helm Repository**:
```bash
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update
```

2. **Create Values File**:
```bash
cat > monitoring-values.yaml << EOF
alertmanager:
  enabled: false
prometheus:
  prometheusSpec:
    service:
      type: NodePort
      nodePort: 30001
    storageSpec:
      volumeClaimTemplate:
        spec:
          storageClassName: standard
          accessModes:
            - ReadWriteOnce
          resources:
            requests:
              storage: 5Gi
grafana:
  enabled: true
  service:
    type: NodePort
    nodePort: 30002
  adminUser: admin
  adminPassword: admin123
EOF
```

3. **Deploy Monitoring Stack**:
```bash
helm upgrade --install monitoring prometheus-community/kube-prometheus-stack \
  -f monitoring-values.yaml \
  -n monitoring \
  --create-namespace
```

4. **Access Dashboards**:
   - **Prometheus**: `http://AGENT_IP:30001`
   - **Grafana**: `http://AGENT_IP:30002` (admin/admin123)

### Monitoring Endpoints

| Service | URL | Purpose |
|---------|-----|---------|
| Prometheus | `http://AGENT_IP:30001` | Metrics Collection |
| Grafana | `http://AGENT_IP:30002` | Visualization |
| Application Health | `http://APP_URL/healthz` | App Health Check |
| Application Ready | `http://APP_URL/ready` | App Readiness |

## 🔒 Security

### Security Scanning Pipeline

The project includes multiple security layers:

1. **SonarQube**: Code quality and security analysis
2. **Trivy**: Container vulnerability scanning
3. **OWASP Dependency Check**: Dependency vulnerability scanning
4. **AWS Security Groups**: Network security

### Security Best Practices

1. **Secrets Management**:
   - Use Kubernetes secrets for sensitive data
   - Never commit secrets to Git
   - Rotate secrets regularly

2. **Network Security**:
   - Security groups restrict access
   - Private subnets for databases
   - TLS encryption for all communications

3. **Container Security**:
   - Regular image scanning
   - Non-root containers
   - Resource limits

4. **Access Control**:
   - IAM roles and policies
   - RBAC in Kubernetes
   - Multi-factor authentication

## 🐛 Troubleshooting

### Common Issues

#### 1. Terraform Apply Fails
```bash
# Check AWS credentials
aws sts get-caller-identity

# Check region
aws configure get region

# Verify key pair exists
aws ec2 describe-key-pairs --key-names devsecops-key
```

#### 2. Jenkins Not Accessible
```bash
# Check security group
aws ec2 describe-security-groups --group-names jenkins-sg

# Check instance status
aws ec2 describe-instances --filters "Name=tag:Name,Values=jenkins-master"

# SSH and check Jenkins service
ssh -i devsecops-key.pem ec2-user@JENKINS_IP
sudo systemctl status jenkins
```

#### 3. Kubernetes Pods Not Starting
```bash
# Check pod logs
kubectl logs -f deployment/api -n three-tier

# Check events
kubectl get events -n three-tier --sort-by='.lastTimestamp'

# Check resources
kubectl top nodes
kubectl top pods -n three-tier
```

#### 4. Application Not Accessible
```bash
# Check ingress
kubectl get ingress -n three-tier

# Check services
kubectl get svc -n three-tier

# Check endpoints
kubectl get endpoints -n three-tier
```

### Debug Commands

```bash
# Infrastructure
terraform plan
terraform state list
terraform output

# Kubernetes
kubectl get all -n three-tier
kubectl describe pod POD_NAME -n three-tier
kubectl logs POD_NAME -n three-tier

# Jenkins
sudo journalctl -u jenkins -f
sudo systemctl restart jenkins

# Docker
docker ps
docker logs CONTAINER_ID
```

### Log Locations

| Component | Log Location |
|-----------|-------------|
| Jenkins | `/var/log/jenkins/jenkins.log` |
| SonarQube | `/opt/sonarqube/logs/` |
| Kubernetes | `kubectl logs` |
| Application | Container logs via kubectl |

## 🤝 Contributing

### How to Contribute

1. **Fork the Repository**
2. **Create Feature Branch**:
```bash
git checkout -b feature/your-feature-name
```

3. **Make Changes and Test**
4. **Commit Changes**:
```bash
git commit -m "Add your feature description"
```

5. **Push to Branch**:
```bash
git push origin feature/your-feature-name
```

6. **Create Pull Request**

### Development Guidelines

- Follow existing code style
- Add tests for new features
- Update documentation
- Test in development environment first

### Reporting Issues

Please include:
- Environment details
- Steps to reproduce
- Expected vs actual behavior
- Relevant logs

## 📞 Support

- **Documentation**: Check this README first
- **Issues**: Create GitHub issue with details
- **Discussions**: Use GitHub Discussions for questions

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🙏 Acknowledgments

- AWS for cloud infrastructure
- Kubernetes community
- Jenkins community
- Open source security tools

---

**⭐ If this project helped you, please give it a star!**

**🔄 Keep this project updated by watching for new releases**
