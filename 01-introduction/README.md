# 📺 AWS Course on YouTube  
🎥 [Watch the full course here](https://youtu.be/R6yysJg_rKE?list=PLJB9b1bbB85EabGxfihssYhe46dZRHXfn)

---

# ☁️ Introduction to AWS

Amazon Web Services (**AWS**) is the world’s leading **cloud computing platform**, offering **200+ fully managed services** for compute, storage, networking, databases, machine learning, DevOps, and more.  

As a **Cloud Infrastructure Engineer**, understanding AWS fundamentals is the first step toward building **scalable, secure, and cost-effective architectures**.

---

## 📌 What is Cloud Computing?

Cloud computing provides **on-demand IT resources** (servers, storage, databases, networking, software) over the internet with **pay-as-you-go pricing**.

- **No upfront costs** – pay only for what you use  
- **Scalability** – scale up/down based on demand  
- **Global reach** – deploy apps close to users  

---

## 📌 AWS Global Infrastructure

AWS is built around **Regions** and **Availability Zones (AZs)**.

- **Region** → A physical location worldwide (e.g., `us-east-1`, `ap-south-1`)  
- **Availability Zone (AZ)** → A physically separate data center within a region (usually 3+ per region)  
- **Edge Locations** → For **CloudFront (CDN)** and caching to improve performance  

Example:  
Region: us-east-1 (N. Virginia)
AZs: us-east-1a, us-east-1b, us-east-1c


---

## 📌 Global vs Regional Services

- **Global Services**: IAM, Route 53, CloudFront, WAF  
- **Regional Services**: EC2, S3, RDS, Lambda, VPC  

---

## 📌 AWS Account Setup

To create AWS account you need dabit or credit card
1. Go to [aws.amazon.com](https://aws.amazon.com/) → **Create an AWS Account**  
2. Provide email, password, and payment details (required even for Free Tier)  
3. Verify with phone number and SMS/Call  
4. Choose **Support Plan** (Basic = Free)  
5. Access AWS Management Console: [console.aws.amazon.com](https://console.aws.amazon.com)  

---

## 📌 Free Tier

AWS provides a **12-month Free Tier** for beginners:

- **EC2** → 750 hours/month of t2.micro or t3.micro  
- **S3** → 5 GB storage  
- **RDS** → 750 hours/month of db.t2.micro  
- **Lambda** → 1M requests/month free  

⚠️ Always track usage with **AWS Billing Dashboard** to avoid unexpected costs.  

---

## 📌 AWS Management Tools

- **AWS Console** → Web-based GUI  
- **AWS CLI** → Command line interface  
- **AWS SDKs** → APIs for programming languages (Python Boto3, Java, Node.js, etc.)  
- **CloudShell** → Browser-based CLI preconfigured with AWS credentials  

---

## ✅ Try it Yourself

1. Sign up for AWS Free Tier.  
2. Explore AWS Console → Navigate to EC2, S3, IAM.  
3. Install AWS CLI and configure it:  
bash
   aws configure


(Enter Access Key, Secret Key, Region, Output format)
4. Run:

aws s3 ls

to list S3 buckets.
