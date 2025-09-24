# 📺 AWS Course on YouTube  
🎥 [Watch the full course here](https://youtu.be/R6yysJg_rKE?list=PLJB9b1bbB85EabGxfihssYhe46dZRHXfn)

---

# ⚖️ Elastic Load Balancing (ELB) & Auto Scaling

High availability and fault tolerance in AWS come from **Load Balancers** and **Auto Scaling Groups (ASG)**.  
They work together to ensure your app is **always up, scalable, and resilient**.

---

## 📌 Elastic Load Balancer (ELB)

Distributes incoming traffic across multiple EC2 instances.

### Types of Load Balancers
1. **Application Load Balancer (ALB)**  
   - Operates at Layer 7 (HTTP/HTTPS).  
   - Supports host/path-based routing, WebSockets.  
   - Best for web apps & microservices.  

2. **Network Load Balancer (NLB)**  
   - Operates at Layer 4 (TCP/UDP).  
   - Handles millions of requests per second.  
   - Best for low-latency, high-performance apps.  

3. **Classic Load Balancer (CLB)**  
   - Legacy, basic Layer 4/7 features.  
   - Not recommended for new apps.  

---

## 📌 Create an Application Load Balancer – Console

1. Go to **EC2 → Load Balancers → Create Load Balancer**.  
2. Choose **Application Load Balancer**.  
3. Configure:
   - Name: `my-alb`  
   - Scheme: Internet-facing  
   - VPC & Subnets: Select at least **2 public subnets**  
   - Security Group: Allow HTTP(80) / HTTPS(443)  
4. Create a **Target Group**:
   - Type: Instances  
   - Protocol: HTTP, Port 80  
   - Register your EC2 instances.  
5. Finish setup → ALB DNS name will be available (e.g., `my-alb-123456.elb.amazonaws.com`).  

---

## 📌 Create an Application Load Balancer – CLI

### 1️⃣ Create Target Group

aws elbv2 create-target-group \
  --name my-targets \
  --protocol HTTP \
  --port 80 \
  --vpc-id vpc-123456

### 2️⃣ Register Instances
aws elbv2 register-targets \
  --target-group-arn arn:aws:elasticloadbalancing:region:acct:targetgroup/my-targets/123456 \
  --targets Id=i-0123456789abcdef0 Id=i-0abcdef1234567890

### 3️⃣ Create Load Balancer
aws elbv2 create-load-balancer \
  --name my-alb \
  --subnets subnet-aaa subnet-bbb \
  --security-groups sg-123456

### 4️⃣ Create Listener
aws elbv2 create-listener \
  --load-balancer-arn arn:aws:elasticloadbalancing:region:acct:loadbalancer/app/my-alb/123456 \
  --protocol HTTP --port 80 \
  --default-actions Type=forward,TargetGroupArn=arn:aws:elasticloadbalancing:region:acct:targetgroup/my-targets/123456

### 📌 Auto Scaling Group (ASG)

Automatically adds/removes EC2 instances based on demand.

Console Steps

Go to EC2 → Auto Scaling Groups → Create Auto Scaling group.

Create a Launch Template:

Choose AMI, instance type, key pair, security group, user data.

Select VPC & Subnets (min 2 for HA).

Attach to an ALB target group (optional, for load balancing).

Configure scaling policy:

Min size: 2

Desired capacity: 2

Max size: 5

Scaling policy: Target 70% CPU utilization.

Review & Launch.

CLI Steps
# 1️⃣ Create Launch Template
aws ec2 create-launch-template \
  --launch-template-name my-template \
  --version-description v1 \
  --launch-template-data '{
    "ImageId":"ami-0abcdef1234567890",
    "InstanceType":"t2.micro",
    "KeyName":"MyKey",
    "SecurityGroupIds":["sg-123456"],
    "UserData":"IyEvYmluL2Jhc2gKeXVtIHVwZGF0ZSAtCnllcyBhcHQgaW5zdGFsbCAtLXllcyBhcGFjaGUy"
  }'

# 2️⃣ Create Auto Scaling Group
aws autoscaling create-auto-scaling-group \
  --auto-scaling-group-name my-asg \
  --launch-template LaunchTemplateName=my-template,Version=1 \
  --min-size 2 --max-size 5 --desired-capacity 2 \
  --vpc-zone-identifier "subnet-aaa,subnet-bbb"

# 3️⃣ Attach to Target Group
aws autoscaling attach-load-balancer-target-groups \
  --auto-scaling-group-name my-asg \
  --target-group-arns arn:aws:elasticloadbalancing:region:acct:targetgroup/my-targets/123456

## ✅ Try it Yourself

Launch 2 EC2 instances with Apache (index.html showing instance ID).

Create an ALB and register these EC2s as targets.

Access ALB DNS name → refresh multiple times → requests should rotate across EC2s.

Create an Auto Scaling Group (min=1, max=3) → stress test CPU → watch new instances launch.

Terminate one instance → ASG automatically replaces it.