# 📺 AWS Course on YouTube  
🎥 [Watch the full course here](https://youtu.be/R6yysJg_rKE?list=PLJB9b1bbB85EabGxfihssYhe46dZRHXfn)

---

# 🌐 AWS Networking – VPC (Virtual Private Cloud)

A **VPC (Virtual Private Cloud)** is a logically isolated section of the AWS cloud where you can **launch resources in a virtual network** that you define.  
It’s the foundation of **networking, security, and connectivity** in AWS.

---

## 📌 VPC Basics

- **Default VPC** → Each region has a default VPC, ready to use.  
- **Custom VPC** → Recommended for production, gives full control over IP ranges, subnets, routing, and security.  

### VPC Components
- **CIDR Block** → Defines IP address range (e.g., `10.0.0.0/16`)  
- **Subnets** → Divide VPC into smaller networks (public/private)  
- **Route Tables** → Rules for directing traffic  
- **Internet Gateway (IGW)** → Provides internet access to public subnets  
- **NAT Gateway** → Allows private subnets to access internet securely  
- **Security Groups (SGs)** → Virtual firewalls at instance level  
- **Network ACLs (NACLs)** → Firewall rules at subnet level  
- **VPC Peering / Transit Gateway** → Connect VPCs together  

---

## 📌 Creating a Custom VPC

### Step 1: Create VPC

aws ec2 create-vpc --cidr-block 10.0.0.0/16

Step 2: Create Subnets
# Public subnet
aws ec2 create-subnet --vpc-id vpc-123456 --cidr-block 10.0.1.0/24 --availability-zone us-east-1a

# Private subnet
aws ec2 create-subnet --vpc-id vpc-123456 --cidr-block 10.0.2.0/24 --availability-zone us-east-1b

Step 3: Create & Attach Internet Gateway
aws ec2 create-internet-gateway
aws ec2 attach-internet-gateway --internet-gateway-id igw-123456 --vpc-id vpc-123456

Step 4: Create Route Table
aws ec2 create-route-table --vpc-id vpc-123456
aws ec2 create-route --route-table-id rtb-123456 --destination-cidr-block 0.0.0.0/0 --gateway-id igw-123456


Associate Route Table with Public Subnet:

aws ec2 associate-route-table --subnet-id subnet-111111 --route-table-id rtb-123456

Step 5: Create NAT Gateway (for private subnet outbound internet)
aws ec2 allocate-address --domain vpc
aws ec2 create-nat-gateway --subnet-id subnet-111111 --allocation-id eipalloc-123456


Update Private Route Table:

aws ec2 create-route --route-table-id rtb-222222 --destination-cidr-block 0.0.0.0/0 --nat-gateway-id nat-123456

## 📌 Security: SG vs NACL

Feature	Security Group (SG)	Network ACL (NACL)
Scope	Instance level	Subnet level
Stateful?	✅ Yes	❌ No (stateless)
Default behavior	Deny all inbound, allow all outbound	Allow all inbound/outbound
Rules	Allow only	Allow & Deny

## 📌 Peering & Transit

VPC Peering → Connect two VPCs privately (point-to-point).

Transit Gateway → Hub-and-spoke model for connecting multiple VPCs and on-premises networks.

## 📌 Connectivity Options

Direct Connect → Dedicated line from on-premises to AWS

VPN → IPSec VPN tunnel from on-premises to VPC

PrivateLink → Private access to AWS services without internet

## ✅ Try it Yourself

Create a custom VPC (10.0.0.0/16) with 1 public and 1 private subnet.

Attach an Internet Gateway and configure a public route table.

Launch an EC2 in the public subnet → test internet access.

Launch an EC2 in the private subnet → test NAT Gateway internet access.

Create Security Groups to allow SSH (22) and HTTP (80).