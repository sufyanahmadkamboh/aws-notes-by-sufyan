# 📺 AWS Course on YouTube  
🎥 [Watch the full course here](https://youtu.be/R6yysJg_rKE?list=PLJB9b1bbB85EabGxfihssYhe46dZRHXfn)

---

# 🏛️ AWS Architecture Design

Designing solutions in AWS requires balancing **performance, security, cost, and resilience**.  
AWS provides the **Well-Architected Framework** to guide engineers.

---

## 📌 AWS Well-Architected Framework

The framework is built on **6 pillars**:

1. **Operational Excellence**  
   - Automate deployments, monitoring, and responses.  
   - Use Infrastructure as Code (IaC).  

2. **Security**  
   - Principle of least privilege (IAM).  
   - Enable encryption (KMS, SSL/TLS).  
   - Monitor with CloudTrail & GuardDuty.  

3. **Reliability**  
   - Design for failure (multi-AZ, multi-region).  
   - Use Auto Scaling & Load Balancers.  
   - Backup & disaster recovery strategies.  

4. **Performance Efficiency**  
   - Use right instance types.  
   - Leverage caching (CloudFront, ElastiCache).  
   - Use managed services.  

5. **Cost Optimization**  
   - Use Spot/Reserved Instances.  
   - S3 lifecycle policies.  
   - Monitor with Cost Explorer.  

6. **Sustainability**  
   - Optimize compute and storage for minimal waste.  
   - Use serverless & managed services.  

---

## 📌 High Availability & Fault Tolerance

- Deploy across **multiple AZs**.  
- Use **ALB/NLB** for load distribution.  
- Enable **Multi-AZ RDS** and **Read Replicas**.  
- For global apps → **Multi-Region failover (Route 53 + CloudFront)**.  

---

## 📌 Scalability & Elasticity

- **Vertical Scaling** → Increase instance size.  
- **Horizontal Scaling** → Add more instances (ASG).  
- **Elastic Load Balancing + Auto Scaling** → Handle variable traffic.  

---

## 📌 Common AWS Architecture Patterns

### 1️⃣ Web Application (3-Tier)
- **Tier 1 (Frontend):** S3 + CloudFront (static website).  
- **Tier 2 (Backend):** EC2/ECS/EKS + ALB.  
- **Tier 3 (Database):** RDS Multi-AZ / DynamoDB.  

### 2️⃣ Serverless Application
- API Gateway → Lambda → DynamoDB.  
- S3 for static hosting.  

### 3️⃣ Data Lake & Analytics
- Ingest: Kinesis / S3  
- Store: S3 + Glue Catalog  
- Analyze: Athena / Redshift  

### 4️⃣ Hybrid Cloud
- On-prem + AWS via Direct Connect / VPN.  
- Use **Outposts** for hybrid workloads.  

---

## 📌 Security in Architecture

- Use **VPC with public/private subnets**.  
- Security Groups + NACLs.  
- WAF + Shield for DDoS protection.  
- Centralized logging with CloudWatch + CloudTrail.  

---

## 🚀 Example: Highly Available Web App

- **Route 53** → DNS.  
- **CloudFront** → CDN + caching.  
- **ALB** → Distribute traffic across AZs.  
- **Auto Scaling Group** → EC2 instances.  
- **RDS Multi-AZ** → Database.  
- **S3 + Lifecycle** → File storage.  
- **CloudWatch** → Monitoring.  

---

## ✅ Try it Yourself

1. Design a **3-tier VPC architecture**:  
   - Public Subnet: ALB + Bastion Host.  
   - Private Subnet: EC2 app servers.  
   - DB Subnet: RDS Multi-AZ.  

2. Deploy a **serverless app** with API Gateway + Lambda + DynamoDB.  

3. Implement **multi-region failover** using Route 53.  

