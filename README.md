# Multi-Cloud Computing — AWS + Azure + GCP

## Course Learning Model

> **Concept → AWS → Azure → GCP → Compare → Hands-on → Troubleshoot → Automate → Project**

The goal is **not** to memorize three different clouds. Students first learn the cloud concept and then understand how each provider implements that concept.

---

# Course Roadmap

| Level | Module             | Major Topics                                 |
| ----- | ------------------ | -------------------------------------------- |
| 1     | Cloud Fundamentals | Cloud models, regions, zones, IaaS/PaaS/SaaS |
| 2     | Compute            | VMs, images, scaling, serverless             |
| 3     | Networking         | VPC/VNet, subnet, routing, NAT, DNS, LB      |
| 4     | Storage            | Object, block, file, archive                 |
| 5     | Databases          | SQL, NoSQL, cache, warehouse                 |
| 6     | Containers         | Docker, registries, container services       |
| 7     | Kubernetes         | EKS, AKS, GKE                                |
| 8     | DevOps             | Git, CI/CD, Jenkins, GitHub Actions          |
| 9     | IAM & Security     | IAM, RBAC, identities, secrets, KMS          |
| 10    | Monitoring         | Metrics, logs, traces, alerts                |
| 11    | Automation         | Terraform, IaC, cloud automation             |
| 12    | Multi-Cloud        | Architecture, governance, DR, cost           |
| 13    | Project            | End-to-end production application            |
| 14    | Interview          | Cloud + DevOps interview preparation         |

---

# Module 1 — Cloud Computing Fundamentals

### Topics

* What is Cloud Computing?
* Traditional Data Center vs Cloud
* Why Cloud?
* Public Cloud
* Private Cloud
* Hybrid Cloud
* Multi-Cloud
* IaaS
* PaaS
* SaaS
* Regions
* Availability Zones
* Global infrastructure
* Scalability
* Elasticity
* High Availability
* Fault Tolerance
* Disaster Recovery
* Pay-as-you-go

### Service Comparison

| Concept           | AWS                  | Azure              | GCP                  |
| ----------------- | -------------------- | ------------------ | -------------------- |
| Cloud Provider    | AWS                  | Microsoft Azure    | Google Cloud         |
| Region            | AWS Region           | Azure Region       | Google Cloud Region  |
| Availability Zone | Availability Zone    | Availability Zone  | Zone                 |
| Account Boundary  | AWS Account          | Azure Subscription | Google Cloud Project |
| Resource Grouping | Tags / Organizations | Resource Groups    | Projects / Folders   |
| CLI               | AWS CLI              | Azure CLI          | gcloud CLI           |

### IaaS / PaaS / SaaS

| Model      | AWS Example          | Azure Example   | GCP Example      |
| ---------- | -------------------- | --------------- | ---------------- |
| IaaS       | EC2                  | Azure VM        | Compute Engine   |
| PaaS       | Elastic Beanstalk    | App Service     | App Engine       |
| Serverless | Lambda               | Azure Functions | Cloud Functions  |
| SaaS       | Amazon WorkMail etc. | Microsoft 365   | Google Workspace |

---

# Module 2 — Compute

## Core Concepts

* CPU
* RAM
* Disk
* VM
* Machine Image
* Instance Types
* Boot Disk
* Public/Private IP
* SSH/RDP
* Scaling
* Auto Scaling
* Load Balancing
* Serverless

## AWS vs Azure vs GCP

| Capability            | AWS                    | Azure                                     | GCP                                   |
| --------------------- | ---------------------- | ----------------------------------------- | ------------------------------------- |
| Virtual Machine       | EC2                    | Azure Virtual Machines                    | Compute Engine                        |
| VM Image              | AMI                    | Managed Image                             | Machine Image                         |
| VM Scale              | EC2 Auto Scaling       | VM Scale Sets                             | Managed Instance Groups               |
| Serverless Function   | Lambda                 | Azure Functions                           | Cloud Run functions / Cloud Functions |
| Application Platform  | Elastic Beanstalk      | App Service                               | App Engine                            |
| Serverless Containers | AWS Fargate            | Azure Container Apps                      | Cloud Run                             |
| Load Balancing        | Elastic Load Balancing | Azure Load Balancer / Application Gateway | Cloud Load Balancing                  |

### Hands-on Labs

1. Create AWS EC2
2. Create Azure VM
3. Create GCP Compute Engine VM
4. Install Linux
5. Connect through SSH
6. Install Nginx
7. Deploy web application
8. Configure firewall
9. Create VM image
10. Configure auto scaling
11. Configure load balancing

---

# Module 3 — Networking

## Concepts

* OSI basics
* TCP/IP
* IPv4
* CIDR
* Public IP
* Private IP
* Subnets
* Routing
* Internet Gateway
* NAT
* Firewall
* Security Groups
* Network ACL
* DNS
* VPN
* Peering
* Load Balancer

## Side-by-Side

| Capability           | AWS                    | Azure                | GCP                  |
| -------------------- | ---------------------- | -------------------- | -------------------- |
| Virtual Network      | VPC                    | VNet                 | VPC                  |
| Subnet               | VPC Subnet             | VNet Subnet          | VPC Subnet           |
| Routing              | Route Tables           | Route Tables         | Routes               |
| Firewall             | Security Groups / NACL | NSG / Azure Firewall | VPC Firewall Rules   |
| NAT                  | NAT Gateway            | NAT Gateway          | Cloud NAT            |
| DNS                  | Route 53               | Azure DNS            | Cloud DNS            |
| VPN                  | AWS Site-to-Site VPN   | VPN Gateway          | Cloud VPN            |
| Peering              | VPC Peering            | VNet Peering         | VPC Network Peering  |
| Dedicated Connection | Direct Connect         | ExpressRoute         | Cloud Interconnect   |
| Load Balancer        | ELB                    | Azure Load Balancer  | Cloud Load Balancing |

### Major Lab

**Build a 3-tier application network**

```text
Internet
   |
Load Balancer
   |
Web Tier
   |
Application Tier
   |
Database Tier
```

Build the same architecture in AWS, Azure and GCP.

---

# Module 4 — Storage

| Storage | AWS        | Azure         | GCP                   |
| ------- | ---------- | ------------- | --------------------- |
| Object  | S3         | Blob Storage  | Cloud Storage         |
| Block   | EBS        | Managed Disks | Persistent Disk       |
| File    | EFS        | Azure Files   | Filestore             |
| Archive | S3 Glacier | Blob Archive  | Cloud Storage Archive |

### Topics

* Object storage
* Block storage
* File storage
* Storage classes
* Lifecycle management
* Versioning
* Replication
* Encryption
* Backup
* Archive
* Data migration

### Labs

* Create bucket
* Upload/download files
* Configure public/private access
* Enable versioning
* Configure lifecycle
* Configure encryption
* Create block disk
* Attach disk to VM
* Configure shared file storage

---

# Module 5 — Databases

## Relational

| Requirement    | AWS                   | Azure                               | GCP                  |
| -------------- | --------------------- | ----------------------------------- | -------------------- |
| Managed SQL    | RDS                   | Azure SQL / Azure Database services | Cloud SQL            |
| PostgreSQL     | RDS/Aurora PostgreSQL | Azure Database for PostgreSQL       | Cloud SQL PostgreSQL |
| Data Warehouse | Redshift              | Synapse Analytics                   | BigQuery             |

## NoSQL

| Requirement | AWS                         | Azure     | GCP       |
| ----------- | --------------------------- | --------- | --------- |
| NoSQL       | DynamoDB                    | Cosmos DB | Firestore |
| Wide-column | DynamoDB / related services | Cosmos DB | Bigtable  |

## Cache

| AWS         | Azure               | GCP         |
| ----------- | ------------------- | ----------- |
| ElastiCache | Azure Managed Redis | Memorystore |

### Concepts

* SQL vs NoSQL
* OLTP
* OLAP
* Primary/Replica
* Read Replica
* Backup
* HA
* Failover
* Scaling
* Multi-region databases

---

# Module 6 — Containers

## Docker

* What is Docker?
* Container vs VM
* Image
* Container
* Dockerfile
* Docker Hub
* Registry
* Volumes
* Networks
* Environment variables
* Container security

## Cloud Comparison

| Capability                   | AWS     | Azure                    | GCP               |
| ---------------------------- | ------- | ------------------------ | ----------------- |
| Container Registry           | ECR     | Azure Container Registry | Artifact Registry |
| Managed Containers           | ECS     | Container Apps           | Cloud Run         |
| Serverless Container Compute | Fargate | Container Apps           | Cloud Run         |

### Lab

```text
Application
     ↓
Dockerfile
     ↓
Docker Image
     ↓
Container Registry
     ↓
Cloud Container Platform
```

Build and deploy the same container to all three clouds.

---

# Module 7 — Kubernetes

## Kubernetes Fundamentals

* Kubernetes architecture
* Control Plane
* Worker Node
* Pod
* ReplicaSet
* Deployment
* Service
* Namespace
* ConfigMap
* Secret
* Labels
* Selectors

## Networking

* ClusterIP
* NodePort
* LoadBalancer
* Ingress
* DNS
* NetworkPolicy

## Storage

* Volume
* PV
* PVC
* StorageClass

## Scaling

* HPA
* VPA
* Cluster Autoscaler

## Production

* Rolling deployment
* Blue/Green
* Canary
* RBAC
* Monitoring
* Logging
* Troubleshooting

### Cloud Mapping

| Kubernetes         | AWS               | Azure               | GCP                  |
| ------------------ | ----------------- | ------------------- | -------------------- |
| Managed Kubernetes | EKS               | AKS                 | GKE                  |
| Registry           | ECR               | ACR                 | Artifact Registry    |
| Load Balancer      | AWS Load Balancer | Azure Load Balancer | Cloud Load Balancing |
| Monitoring         | CloudWatch        | Azure Monitor       | Cloud Monitoring     |

### Major Project

Deploy a Node.js application to:

**EKS → AKS → GKE**

---

# Module 8 — DevOps

## Topics

* DevOps fundamentals
* Git
* GitHub
* Git branching
* Pull Requests
* CI
* CD
* Build
* Test
* Artifact
* Deployment
* Release
* Rollback

## Tools

* GitHub Actions
* Jenkins
* Docker
* Kubernetes
* Terraform

## Cloud DevOps Comparison

| Capability | AWS                      | Azure                              | GCP               |
| ---------- | ------------------------ | ---------------------------------- | ----------------- |
| CI/CD      | CodePipeline / CodeBuild | Azure DevOps                       | Cloud Build       |
| Deployment | CodeDeploy               | Azure DevOps / deployment services | Cloud Deploy      |
| Registry   | ECR                      | ACR                                | Artifact Registry |
| Artifacts  | CodeArtifact             | Azure Artifacts                    | Artifact Registry |
| Secrets    | Secrets Manager          | Key Vault                          | Secret Manager    |

### Multi-Cloud CI/CD

```text
Developer
    |
   Git
    |
 GitHub
    |
 GitHub Actions
    |
    +------ AWS
    |
    +------ Azure
    |
    +------ GCP
```

---

# Module 9 — IAM & Security

## Concepts

* Authentication
* Authorization
* IAM
* Users
* Groups
* Roles
* Policies
* RBAC
* MFA
* Least Privilege
* Service Identity
* Workload Identity
* Secrets
* Encryption
* KMS
* Compliance

## Comparison

| Security          | AWS                             | Azure                        | GCP                     |
| ----------------- | ------------------------------- | ---------------------------- | ----------------------- |
| IAM               | AWS IAM                         | Azure RBAC / Microsoft Entra | Cloud IAM               |
| Identity          | IAM / related identity services | Microsoft Entra ID           | Cloud Identity          |
| Keys              | KMS                             | Key Vault                    | Cloud KMS               |
| Secrets           | Secrets Manager                 | Key Vault                    | Secret Manager          |
| Security Platform | Security Hub                    | Defender for Cloud           | Security Command Center |

Important teaching point:

> **IAM services are not exact copies across clouds. Compare the security concepts first.**

---

# Module 10 — Monitoring & Logging

| Capability | AWS               | Azure                | GCP                     |
| ---------- | ----------------- | -------------------- | ----------------------- |
| Monitoring | CloudWatch        | Azure Monitor        | Cloud Monitoring        |
| Logging    | CloudWatch Logs   | Azure Monitor Logs   | Cloud Logging           |
| Tracing    | X-Ray             | Application Insights | Cloud Trace             |
| Alerts     | CloudWatch Alarms | Azure Monitor Alerts | Cloud Monitoring Alerts |

### Teach

* Metrics
* Logs
* Traces
* Dashboards
* Alerts
* SLI
* SLO
* Incident response
* Application monitoring
* Infrastructure monitoring
* Kubernetes monitoring

### Troubleshooting Labs

* High CPU
* Memory issue
* Pod CrashLoopBackOff
* ImagePullBackOff
* Service unavailable
* HTTP 500
* DNS failure
* Load balancer failure

---

# Module 11 — Automation & Infrastructure as Code

## Terraform

Teach:

```text
Terraform
   |
   +--- AWS Provider
   |
   +--- Azure Provider
   |
   +--- GCP Provider
```

### Topics

* Terraform installation
* Providers
* Resources
* Variables
* Outputs
* Modules
* State
* Remote State
* Backend
* Workspaces
* Plan
* Apply
* Destroy
* Terraform with CI/CD

### Cloud Comparison

| IaC             | AWS            | Azure     | GCP                                 |
| --------------- | -------------- | --------- | ----------------------------------- |
| Native IaC      | CloudFormation | ARM/Bicep | Google Cloud infrastructure tooling |
| Multi-Cloud IaC | Terraform      | Terraform | Terraform                           |

---

# Module 12 — Multi-Cloud Architecture

Teach students to design:

### Architecture A — AWS

```text
Route 53
   ↓
Load Balancer
   ↓
EKS
   ↓
RDS
   ↓
S3
```

### Architecture B — Azure

```text
Azure DNS
   ↓
Azure Load Balancer
   ↓
AKS
   ↓
Azure Database
   ↓
Blob Storage
```

### Architecture C — GCP

```text
Cloud DNS
   ↓
Cloud Load Balancing
   ↓
GKE
   ↓
Cloud SQL
   ↓
Cloud Storage
```

### Architecture D — Multi-Cloud

```text
                 Users
                   |
          Global Traffic Layer
                   |
       +-----------+-----------+
       |           |           |
      AWS        Azure        GCP
       |           |           |
      EKS         AKS         GKE
       |           |           |
      RDS      Azure DB     Cloud SQL
```

Discuss:

* Vendor lock-in
* Cost
* Availability
* Disaster recovery
* Security
* Governance
* Networking complexity
* Data movement
* Latency
* Observability
* Skills requirements

---

# Module 13 — Real-World Project

## Multi-Cloud Quick Commerce Application

Build a realistic application containing:

* Frontend
* Node.js backend
* REST API
* PostgreSQL
* Redis
* Docker
* Kubernetes
* Object storage
* CI/CD
* Terraform
* IAM
* Monitoring
* Logging

### AWS Deployment

| Component          | AWS        |
| ------------------ | ---------- |
| Kubernetes         | EKS        |
| Database           | RDS        |
| Container Registry | ECR        |
| Object Storage     | S3         |
| IAM                | IAM        |
| Monitoring         | CloudWatch |

### Azure Deployment

| Component          | Azure                 |
| ------------------ | --------------------- |
| Kubernetes         | AKS                   |
| Database           | Azure Database        |
| Container Registry | ACR                   |
| Object Storage     | Blob Storage          |
| Identity           | Entra ID / Azure RBAC |
| Monitoring         | Azure Monitor         |

### GCP Deployment

| Component          | GCP               |
| ------------------ | ----------------- |
| Kubernetes         | GKE               |
| Database           | Cloud SQL         |
| Container Registry | Artifact Registry |
| Object Storage     | Cloud Storage     |
| IAM                | Cloud IAM         |
| Monitoring         | Cloud Monitoring  |

---

# Standard Lesson Format

Every individual service should follow this exact structure:

## 1. What is it?

Simple beginner explanation.

## 2. Why do we need it?

Explain the business problem.

## 3. How does it work?

Explain architecture and data flow.

## 4. AWS

Service name + architecture + implementation.

## 5. Azure

Equivalent service + architecture + implementation.

## 6. GCP

Equivalent service + architecture + implementation.

## 7. Comparison

| Feature | AWS | Azure | GCP |
| ------- | --- | ----- | --- |

## 8. Hands-on Lab

Provide step-by-step implementation.

## 9. CLI

Provide relevant commands.

## 10. Verification

Show expected output.

## 11. Troubleshooting

Common problems and solutions.

## 12. Real-world Scenario

Explain how companies use the service.

## 13. Interview Questions

Provide beginner, intermediate and advanced questions.

---

# Master Service Comparison Cheat Sheet

The final course should include a large reference table like this:

| Category   | Capability   | AWS                    | Azure                | GCP                  |
| ---------- | ------------ | ---------------------- | -------------------- | -------------------- |
| Compute    | VM           | EC2                    | Azure VM             | Compute Engine       |
| Compute    | Serverless   | Lambda                 | Azure Functions      | Cloud Functions      |
| Compute    | App Platform | Elastic Beanstalk      | App Service          | App Engine           |
| Network    | VPC          | VPC                    | VNet                 | VPC                  |
| Network    | DNS          | Route 53               | Azure DNS            | Cloud DNS            |
| Network    | NAT          | NAT Gateway            | NAT Gateway          | Cloud NAT            |
| Network    | VPN          | Site-to-Site VPN       | VPN Gateway          | Cloud VPN            |
| Storage    | Object       | S3                     | Blob                 | Cloud Storage        |
| Storage    | Block        | EBS                    | Managed Disk         | Persistent Disk      |
| Storage    | File         | EFS                    | Azure Files          | Filestore            |
| Database   | SQL          | RDS                    | Azure SQL            | Cloud SQL            |
| Database   | NoSQL        | DynamoDB               | Cosmos DB            | Firestore            |
| Database   | Warehouse    | Redshift               | Synapse              | BigQuery             |
| Container  | Registry     | ECR                    | ACR                  | Artifact Registry    |
| Container  | Managed      | ECS                    | Container Apps       | Cloud Run            |
| Kubernetes | Managed K8s  | EKS                    | AKS                  | GKE                  |
| DevOps     | CI/CD        | CodePipeline/CodeBuild | Azure DevOps         | Cloud Build          |
| Security   | IAM          | AWS IAM                | Azure RBAC/Entra     | Cloud IAM            |
| Security   | Secrets      | Secrets Manager        | Key Vault            | Secret Manager       |
| Security   | KMS          | KMS                    | Key Vault            | Cloud KMS            |
| Monitoring | Metrics      | CloudWatch             | Azure Monitor        | Cloud Monitoring     |
| Logging    | Logs         | CloudWatch Logs        | Azure Monitor Logs   | Cloud Logging        |
| Tracing    | Tracing      | X-Ray                  | Application Insights | Cloud Trace          |
| IaC        | Native       | CloudFormation         | Bicep                | Google Cloud tooling |
| IaC        | Multi-Cloud  | Terraform              | Terraform            | Terraform            |

**Important:** this table should be treated as a learning map, not as a claim that every service is a perfect 1:1 equivalent. Where an exact equivalent does not exist, the curriculum should explicitly say **“No exact 1:1 equivalent.”**

