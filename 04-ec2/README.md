# 📺 AWS Course on YouTube  
🎥 [Watch the full course here](https://youtu.be/R6yysJg_rKE?list=PLJB9b1bbB85EabGxfihssYhe46dZRHXfn)

---

# 💻 Amazon EC2 (Elastic Compute Cloud)

Amazon **EC2 (Elastic Compute Cloud)** provides resizable virtual servers in the cloud.  
It’s one of the core AWS services used to run applications, host websites, and deploy workloads.

---

## 📌 Key Concepts

- **AMI (Amazon Machine Image)** → Preconfigured OS images (Amazon Linux, Ubuntu, Windows, custom AMIs).  
- **Instance Types** → Define CPU, memory, storage, network (e.g., `t2.micro`, `m5.large`).  
- **Key Pairs** → SSH keys for secure login.  
- **EBS (Elastic Block Store)** → Persistent storage volumes attached to EC2.  
- **Elastic IP** → Static public IP address for EC2.  
- **Security Groups** → Virtual firewall for inbound/outbound rules.  
- **User Data** → Bootstrapping scripts executed when instance launches.  

---

## 📌 Launching an EC2 Instance – AWS Console

1. **Login** to [AWS Console](https://console.aws.amazon.com).  
2. Go to **EC2 → Instances → Launch Instance**.  
3. Configure:
   - **Name & Tags** → e.g., `MyWebServer`.  
   - **AMI** → Choose Amazon Linux 2 or Ubuntu.  
   - **Instance Type** → Select `t2.micro` (Free Tier eligible).  
   - **Key Pair** → Create or use existing (download `.pem` file).  
   - **Network Settings**:
     - VPC → Select default/custom.  
     - Subnet → Public subnet.  
     - Enable **Auto-assign Public IP**.  
     - Security Group → Allow SSH (22) + HTTP (80).  
   - **Storage** → 8–30 GB (default is fine).  
   - **User Data (Optional)** → Run script on startup:
     ```bash
     #!/bin/bash
     yum update -y
     yum install -y httpd
     systemctl start httpd
     systemctl enable httpd
     echo "Hello from EC2 $(hostname)" > /var/www/html/index.html
     ```
4. Click **Launch Instance**.  
5. Copy the **Public IP** and SSH into it:
   ```bash
   ssh -i my-key.pem ec2-user@<PUBLIC_IP>

## 📌 Launching an EC2 Instance – AWS CLI

# 1️⃣ Create a Key Pair
aws ec2 create-key-pair --key-name MyKey --query 'KeyMaterial' --output text > MyKey.pem
chmod 400 MyKey.pem

# 2️⃣ Create a Security Group
aws ec2 create-security-group --group-name MySG --description "My security group" --vpc-id vpc-123456


Allow SSH + HTTP:

aws ec2 authorize-security-group-ingress --group-name MySG --protocol tcp --port 22 --cidr 0.0.0.0/0
aws ec2 authorize-security-group-ingress --group-name MySG --protocol tcp --port 80 --cidr 0.0.0.0/0

# 3️⃣ Launch Instance
aws ec2 run-instances \
  --image-id ami-0abcdef1234567890 \   # Replace with valid AMI ID
  --count 1 \
  --instance-type t2.micro \
  --key-name MyKey \
  --security-groups MySG \
  --user-data file://userdata.sh


Check running instances:

aws ec2 describe-instances --query "Reservations[*].Instances[*].{ID:InstanceId,State:State.Name,IP:PublicIpAddress}"

## 📌 Elastic IP (Static Public IP)

Console:

Go to EC2 → Elastic IPs → Allocate Elastic IP.

Select it → Associate Elastic IP with your instance.

CLI:
aws ec2 allocate-address --domain vpc
aws ec2 associate-address --instance-id i-123456 --allocation-id eipalloc-123456

## 📌 Volumes & Snapshots

Volumes (EBS) → Persistent block storage for EC2.

Snapshots → Backups of volumes stored in S3.

Console:

Go to EC2 → Volumes → Create Volume / Attach Volume.

Take snapshots under Snapshots tab.

CLI:
# Create a 10GB EBS volume
aws ec2 create-volume --availability-zone us-east-1a --size 10 --volume-type gp2

# Create a snapshot of volume
aws ec2 create-snapshot --volume-id vol-123456 --description "Backup"

## 📌 Auto Scaling & Load Balancing

Auto Scaling Group (ASG) → Scale EC2 instances automatically based on demand.

Elastic Load Balancer (ELB/ALB) → Distribute traffic across instances.

Console steps:

Go to EC2 → Auto Scaling Groups.

Create launch template → Select AMI + instance type + key pair.

Configure scaling policy → Target CPU utilization 70%.

Attach ALB to distribute traffic.

## ✅ Try it Yourself

Launch an Ubuntu EC2 in AWS Console with a security group allowing SSH (22).

Connect via SSH:

ssh -i MyKey.pem ubuntu@<PublicIP>


Install Apache:

sudo apt update && sudo apt install -y apache2
sudo systemctl start apache2


Open browser → http://<PublicIP> → You should see the default Apache page.

Create an Elastic IP and assign it to your instance.