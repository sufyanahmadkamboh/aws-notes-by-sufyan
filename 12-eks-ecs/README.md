# 📺 AWS Course on YouTube  
🎥 [Watch the full course here](https://youtu.be/R6yysJg_rKE?list=PLJB9b1bbB85EabGxfihssYhe46dZRHXfn)

---

# 🐳 Containers on AWS – ECS, EKS & Fargate

AWS offers multiple ways to run **containers at scale**:

- **ECS (Elastic Container Service)** → AWS-managed container orchestration.  
- **EKS (Elastic Kubernetes Service)** → Managed Kubernetes clusters.  
- **Fargate** → Serverless compute for ECS/EKS (no EC2 to manage).  

---

## 📌 ECS (Elastic Container Service)

ECS manages Docker containers on AWS.  
Two launch types:  
- **EC2 Launch Type** → Containers run on EC2 instances.  
- **Fargate Launch Type** → Serverless, AWS manages compute.  

### 🚀 Create ECS Service – Console

1. Go to **ECS → Clusters → Create Cluster**.  
   - Choose **Networking only (Fargate)** or **EC2 Linux + Networking**.  
2. Create a **Task Definition**:  
   - Name: `nginx-task`  
   - Launch type: Fargate  
   - Container image: `nginx:latest`  
   - Port: `80`  
   - Memory/CPU: 512 MB, 0.25 vCPU  
3. Create a **Service**:  
   - Launch 2 tasks.  
   - Attach to **ALB** for traffic distribution.  
4. Open ALB DNS → Nginx default page appears.  

---

### 🚀 Create ECS Service – CLI


# Create cluster
aws ecs create-cluster --cluster-name my-cluster

# Register task definition
aws ecs register-task-definition \
  --family nginx-task \
  --network-mode awsvpc \
  --requires-compatibilities FARGATE \
  --cpu "256" --memory "512" \
  --container-definitions '[{
    "name":"nginx",
    "image":"nginx:latest",
    "portMappings":[{"containerPort":80,"hostPort":80}]
  }]'

# Run task
aws ecs run-task \
  --cluster my-cluster \
  --launch-type FARGATE \
  --network-configuration "awsvpcConfiguration={subnets=[subnet-abc],securityGroups=[sg-123],assignPublicIp=ENABLED}" \
  --task-definition nginx-task

## 📌 EKS (Elastic Kubernetes Service)

EKS provides a managed Kubernetes control plane.
You deploy workloads as Kubernetes manifests, AWS manages master nodes.

🚀 Create EKS Cluster – Console

Go to EKS → Create Cluster.

Name: my-eks

Select VPC & subnets.

Create IAM role for EKS cluster.

Launch EKS Node Group (managed EC2 worker nodes).

Update local kubeconfig:

aws eks update-kubeconfig --region us-east-1 --name my-eks


Verify cluster:

kubectl get nodes


Deploy Nginx:

kubectl create deployment nginx --image=nginx
kubectl expose deployment nginx --port=80 --type=LoadBalancer

## 🚀 Create EKS Cluster – CLI
# Create EKS cluster (control plane only)
aws eks create-cluster \
  --name my-eks \
  --role-arn arn:aws:iam::123456789012:role/EKSRole \
  --resources-vpc-config subnetIds=subnet-aaa,subnet-bbb,securityGroupIds=sg-123456

# Update kubeconfig
aws eks update-kubeconfig --name my-eks --region us-east-1


Then deploy workloads with kubectl.

## 📌 Fargate

Serverless compute for containers.

Works with ECS and EKS.

No need to manage EC2 nodes.

Pay only for CPU & memory per second.

Example – Run Nginx on Fargate (Console)

Go to ECS → Create Task Definition.

Select Fargate.

Define container: nginx:latest, port 80.

Run task in VPC public subnet with public IP.

Access via public IP → Nginx welcome page.

## ✅ Try it Yourself

Create an ECS cluster with a Fargate service running Nginx.

Create an EKS cluster → Deploy Nginx via kubectl.

Expose it with a LoadBalancer service.

Compare ECS vs EKS workflows.

Run a container with Fargate (no servers).