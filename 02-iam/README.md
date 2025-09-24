# 📺 AWS Course on YouTube  
🎥 [Watch the full course here](https://youtu.be/R6yysJg_rKE?list=PLJB9b1bbB85EabGxfihssYhe46dZRHXfn)

---

# 🔑 AWS Identity & Access Management (IAM)

**IAM** (Identity & Access Management) is the foundation of AWS security.  
It lets you **create users, groups, and roles** and assign fine-grained **permissions** using **policies**.

---

## 📌 Why IAM?

- Securely manage access to AWS resources  
- Apply the **Principle of Least Privilege** (only required permissions)  
- Use **temporary credentials** for apps (via IAM Roles)  
- Protect AWS accounts with **MFA**  

---

## 📌 Key IAM Components

- **Users** → Individuals/services needing access  
- **Groups** → Collection of users with shared permissions  
- **Roles** → Temporary access, assumed by users/apps/services  
- **Policies** → JSON documents defining permissions  
- **Root User** → The account owner (⚠️ never use daily)  

---

## 📌 IAM Best Practices

- Do NOT use the **root account** for daily tasks  
- Enable **MFA** (Multi-Factor Authentication) for root and IAM users  
- Assign permissions via **groups**, not directly to users  
- Use **IAM roles** for EC2, Lambda, ECS instead of embedding keys  
- Regularly rotate credentials  

---

## 📌 Creating IAM Users & Groups

### 1️⃣ Create a Group (e.g., Admins)

aws iam create-group --group-name Admins

### 2️⃣ Attach a Policy to the Group
aws iam attach-group-policy \
  --group-name Admins \
  --policy-arn arn:aws:iam::aws:policy/AdministratorAccess

### 3️⃣ Create a User
aws iam create-user --user-name alice

## 4️⃣ Add User to Group
aws iam add-user-to-group --user-name alice --group-name Admins

## 📌 IAM Roles

IAM Roles provide temporary security credentials via STS (Security Token Service).

Example: Attach an IAM Role to an EC2 instance to allow it to access S3 without storing access keys.

aws iam create-role --role-name S3ReadOnlyRole \
  --assume-role-policy-document file://trust-policy.json


Example trust-policy.json:

{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": { "Service": "ec2.amazonaws.com" },
      "Action": "sts:AssumeRole"
    }
  ]
}


Attach S3 policy:

aws iam attach-role-policy \
  --role-name S3ReadOnlyRole \
  --policy-arn arn:aws:iam::aws:policy/AmazonS3ReadOnlyAccess

## 📌 IAM Policies (JSON Structure)

A typical policy has:

{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "s3:ListBucket",
        "s3:GetObject"
      ],
      "Resource": [
        "arn:aws:s3:::mybucket",
        "arn:aws:s3:::mybucket/*"
      ]
    }
  ]
}


Effect: Allow / Deny

Action: API calls (s3:GetObject, ec2:StartInstances)

Resource: ARN of resource

Condition (optional): Extra restrictions

## 📌 Multi-Factor Authentication (MFA)

Enable MFA for root and IAM users:

Go to IAM Console → Users → Security Credentials

Assign Virtual MFA device (Google Authenticator, Authy, etc.)

Confirm with OTP codes

CLI Example:

aws iam enable-mfa-device \
  --user-name alice \
  --serial-number arn:aws:iam::123456789012:mfa/alice \
  --authentication-code1 123456 \
  --authentication-code2 654321

## ✅ Try it Yourself

Create a user devuser and add to group Developers.

Attach AmazonS3ReadOnlyAccess policy to the group.

Create an IAM role for EC2 with S3 read-only permissions.

Enable MFA for the root account.

Use aws sts assume-role to test temporary credentials.
