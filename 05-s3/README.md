# 📺 AWS Course on YouTube  
🎥 [Watch the full course here](https://youtu.be/R6yysJg_rKE?list=PLJB9b1bbB85EabGxfihssYhe46dZRHXfn)

---

# 📦 Amazon S3 (Simple Storage Service)

Amazon **S3 (Simple Storage Service)** is an object storage service that lets you store and retrieve unlimited data from anywhere on the web.  
It is designed for **scalability, durability (11 9’s), and security**.

---

## 📌 Key Features

- **Buckets** → Containers for storing objects.  
- **Objects** → Files stored in S3 (with metadata).  
- **Global namespace** → Bucket names must be unique globally.  
- **Durability** → 99.999999999% (11 9’s).  
- **Classes** → Standard, IA (Infrequent Access), Glacier (Archival).  
- **Security** → IAM Policies, Bucket Policies, ACLs.  
- **Static Website Hosting** → Serve web pages directly from S3.  

---

## 📌 Creating a Bucket – Console

1. Open **AWS Console → S3 → Create bucket**.  
2. Enter a globally unique **bucket name** (e.g., `sufyan-demo-bucket`).  
3. Select **Region** (close to your users).  
4. Options:
   - **Block Public Access** (recommended ON, unless for public website).  
   - Enable **Versioning** (optional).  
   - Enable **Server-side encryption** (SSE-S3 or SSE-KMS).  
5. Click **Create bucket**.  

Upload objects:  
- Select bucket → **Upload → Add files → Upload**.  

---

## 📌 Creating a Bucket – CLI


# Create bucket (region: us-east-1)
aws s3api create-bucket --bucket sufyan-demo-bucket --region us-east-1

# Create bucket in another region (e.g., ap-south-1)
aws s3api create-bucket --bucket sufyan-demo-bucket --region ap-south-1 --create-bucket-configuration LocationConstraint=ap-south-1

Upload files:

aws s3 cp file.txt s3://sufyan-demo-bucket/


Download files:

aws s3 cp s3://sufyan-demo-bucket/file.txt ./file.txt


List buckets:

aws s3 ls

## 📌 Bucket Policies (Access Control)
Public Read Policy
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": "*",
      "Action": ["s3:GetObject"],
      "Resource": ["arn:aws:s3:::sufyan-demo-bucket/*"]
    }
  ]
}


Apply via Console: Bucket → Permissions → Bucket Policy.

## 📌 Versioning & Lifecycle Rules

Versioning → Keep multiple versions of an object.

Lifecycle Rules → Automatically transition or delete objects.

Console:

Go to Bucket → Management → Lifecycle rules → Add rule.

Example: Move objects to Glacier after 30 days.

CLI:

aws s3api put-bucket-versioning --bucket sufyan-demo-bucket --versioning-configuration Status=Enabled

## 📌 Static Website Hosting

Console → Select bucket → Properties → Static website hosting.

Enable and set:

Index document: index.html

Error document: error.html

Upload your HTML files.

Copy the endpoint URL and open in browser.

### ⚠️ Must allow public read access with bucket policy.

## 📌 S3 Storage Classes
Storage Class	Use Case	Cost
Standard	Frequent access	💲💲
Intelligent-Tiering	Unknown access patterns	💲+automation
Standard-IA	Infrequent access	💲
One Zone-IA	Infrequent + 1 AZ only	💲
Glacier / Deep Archive	Long-term archival	Lowest 💲
## 📌 Encryption

SSE-S3 → Managed by AWS (AES-256).

SSE-KMS → AWS Key Management Service.

SSE-C → Customer-provided keys.

Enable in Bucket → Properties → Default encryption.

## ✅ Try it Yourself

Create a bucket my-first-bucket in your region.

Upload index.html and enable static website hosting.

Apply a bucket policy to allow public read.

Access via bucket endpoint in browser.

Enable versioning and upload multiple versions of a file.

Add a lifecycle rule to move old objects to Glacier.