# PROPOSAL A — High Efficiency & High Reliability (Higher Cost)

**Cloud Provider:** **AWS**  
Reason:
- EKS of AWS abstracts away most of the hard word
- Excellent CI/CD pipeline
- Strong security
- Easy to transition into multi cloud with TF and cotainers.
## 1. Architecture Overview

### Compute

Amazon EKS for the following reasons:
- Highly available
- Fully managed
- We can have separate namespaces in one cluster for frontend and backend.
- Auto-scaling
### Container Registry

- Amazon ECR (Private Container Registry)
    - Private, encrypted images.
    - Supports lifecycle policies for cleanup.
### Database

Amazon RDS for PSQL:
- Fully managed backups (no manual labor).
- Automated minor upgrades
- HA and point-in-time restore.
### Networking

AWS VPC with 3-tier architecture for security:
- Public subnet with Load Balancer
- Private subnet where the EKS and RDS is deployed

AWS ALB (Application Load Balancer):
- Routes to front-end and backend services
### Storage

S3 buckets for storage of static files and assets.
### Secrets Management

AWS Secrets Manager
- Rotates DB passwords.
-  No hardcoded credentials.
### Monitoring

- Amazon Cloudwatch + Container Insights.
- AWS X-Ray for tracing.
- AWS GuardDuty for security threat detection.
## 2. Code & Repositories

For privacy of our code:
- Gitea (secure but adds overhead)
- GitHub Enterprise Cloud (Private repos but still hosted on MS servers)

## 3. CI/CD Pipeline

### Tools

- GitHub Actions or AWS CodePipeline + CodeBuild
### Pipeline Flow

1. Developer pushes to `dev` branch
2. CI pipeline runs:
    - Install dependencies
    - Run unit tests
    - Build Docker images
    - Push to ECR
3. CD pipeline deploys automatically to EKS dev namespace
4. Manual approval gate for production
5. After approval → deploy to EKS prod namespace

## 4. Security

- Private repos
- No credentials stored in code
- RBAC in Kubernetes
- IAM roles for service accounts
- ALB with HTTPS enabled
- RDS deployed in private subnet
- Production deployment restricted to specific IAM users
## 5. Cost Justification

- Managed Kubernetes (EKS) and RDS Multi-AZ are costlier but provide:
    - High uptime
    - Automatic scaling
    - Enterprise-level reliability
    - Minimal maintenance burden    

# PROPOSAL B: Low Cost, Simpler Setup (Lower Efficiency but Affordable)

**Cloud Provider:** **Azure** (or AWS/GCP - but Azure chosen for lower baseline cost with AKS & managed PostgreSQL)

---

## 1. Architecture Overview

### Compute

- Azure Kubernetes Service (AKS — Basic Tier)
    - Single node pool
    - Minimal auto-scaling
    - No multi-AZ redundancy
    - Runs frontend + backend in a single cluster
### Container Registry

- Azure Container Registry (ACR — Basic SKU)
    - Private, low-cost registry

### Database

Option A (managed but cheaper):
- **Azure PostgreSQL Flexible Server (Single Zone, Burstable Tier)**

Option B (cheapest):
- **PostgreSQL container inside AKS**
    - No HA
    - Manual backups
    - Good for low-budget prototype
### Networking
- Basic VNet
- Nginx Ingress Controller inside AKS
- Single public IP for all services

### Storage

- Azure Disk for DB (if self-hosted)
- Azure Files for static assets

### Secrets Management

- Azure Key Vaults
- K8s Secrets (Base64-encoded, not highly secure but acceptable)

### Monitoring

- Basic Azure Monitor
- Log Analytics optional (cost-saving)

## 2. Code & Repositories

- **Gitea Hosted on a Small VM**
    - Fully private
    - Zero per-user cost
    - Ideal for budget-friendly setups

Why gitea:
- Self-hosted
- Cheap
- Doesn’t expose code publicly
- Still supports webhooks for CI/CD
## 3. CI/CD Pipeline

- **Azure DevOps** (Free Tier) OR **GitHub Actions Free tier**
### Pipeline Flow

1. Push to repository triggers pipeline
2. Pipeline builds Docker images
3. Push image to ACR
4. Deploy to AKS dev namespace
5. Production deployment requires manual approval
### Justifying:

- Azure DevOps free minutes
- AKS basic tier
- No expensive monitoring or multi-AZ
- Optional self-hosted DB reduces cost significantly
## 4. Security

- Private Gitea repository
- No hardcoding secrets
- Minimal RBAC
- Production deployment restricted to 1–2 DevOps users
- HTTPS via free Let’s Encrypt ingress certificate
## 5. Cost Justification

- AKS basic tier + ACR basic SKU = very low monthly cost
- Self-hosted Gitea = free
- Optional self-hosted PostgreSQL = free
- Lower reliability but meets functional needs
This proposal focuses on **cost reduction**, while still keeping containerization and CI/CD.

# Five (5) Discovery Questions for the Client


1. Which team members should have access to production, and what level of control do they need?  
	Helps build IAM/RBAC roles for Git, CI/CD, and cloud.
2. Do you need logging and monitoring for audits or compliance?  
    To decide whether to use basic logs or advanced monitoring stacks.
3. What is your expected incident response plan? Do you need automated alerts for downtime or security events?  
	Helps decide monitoring tools, alerting pipelines, and SLAs.
4. What type of data is to be stored in the database?
    Determines encryption, security and compliance with laws and regulations.