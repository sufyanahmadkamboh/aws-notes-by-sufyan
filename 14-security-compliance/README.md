# 📺 AWS Course on YouTube  
🎥 [Watch the full course here](https://youtu.be/R6yysJg_rKE?list=PLJB9b1bbB85EabGxfihssYhe46dZRHXfn)

---

# 🔐 AWS Security & Compliance

Security in AWS is based on a **shared responsibility model**:  
- **AWS** secures the cloud infrastructure.  
- **You** secure your applications, data, and IAM configurations.  

This section covers:  
- IAM Best Practices  
- KMS (Key Management Service)  
- GuardDuty  
- Security Hub  
- WAF (Web Application Firewall)  

---

## 📌 IAM Best Practices

- Enable **MFA** for root & IAM users.  
- Don’t use **root account** for daily tasks.  
- Use **IAM roles** instead of long-term access keys.  
- Apply **least privilege principle** → give only needed permissions.  
- Use **IAM groups** to manage permissions.  
- Enable **AWS IAM Access Analyzer** to detect broad permissions.  

### Console
1. Go to **IAM → Users → Add User**.  
2. Assign to **Group** with restricted policies.  
3. Enable **MFA** under Security Credentials.  

### CLI

aws iam create-user --user-name devuser
aws iam create-group --group-name DevTeam
aws iam add-user-to-group --user-name devuser --group-name DevTeam

📌 KMS (Key Management Service)

AWS KMS manages encryption keys for data at rest and in transit.

Console

Go to KMS → Create Key.

Choose Symmetric/Asymmetric.

Define key admins & usage permissions.

Use with S3, EBS, RDS, Lambda (built-in).

CLI
# Create KMS Key
aws kms create-key --description "My encryption key"

# Encrypt a string
aws kms encrypt \
  --key-id alias/mykey \
  --plaintext "MySecretData" \
  --output text --query CiphertextBlob

## 📌 GuardDuty (Threat Detection)

GuardDuty is a threat detection service that monitors accounts, logs, and network traffic.

Detects suspicious activity like unusual API calls, brute-force attacks, or exfiltration attempts.

Console

Go to GuardDuty → Enable GuardDuty.

Review findings (alerts).

CLI
aws guardduty create-detector --enable
aws guardduty list-findings --detector-id <id>

## 📌 Security Hub (Centralized Compliance)

Security Hub provides a single dashboard for compliance and security findings.

Integrates with GuardDuty, Inspector, Macie, and 3rd-party tools.

Follows compliance standards (CIS, PCI DSS, ISO).

Console

Go to Security Hub → Enable Security Hub.

View compliance checks across all accounts.

CLI
aws securityhub enable-security-hub
aws securityhub get-findings

## 📌 AWS WAF (Web Application Firewall)

Protects web apps from SQL injection, XSS, DDoS, bad bots.

Console

Go to WAF & Shield → Web ACLs → Create Web ACL.

Associate with CloudFront, ALB, or API Gateway.

Add rules:

Block SQL injection.

Allow only specific IP ranges.

CLI
aws wafv2 create-web-acl \
  --name mywebacl \
  --scope REGIONAL \
  --default-action Allow={} \
  --rules '[
    {
      "Name":"BlockSQLi",
      "Priority":1,
      "Action":{"Block":{}},
      "Statement":{"SqliMatchStatement":{"FieldToMatch":{"Body":{}},"TextTransformations":[{"Priority":0,"Type":"NONE"}]}},
      "VisibilityConfig":{"SampledRequestsEnabled":true,"CloudWatchMetricsEnabled":true,"MetricName":"BlockSQLi"}
    }
  ]' \
  --visibility-config SampledRequestsEnabled=true,CloudWatchMetricsEnabled=true,MetricName=MyWebACL

## ✅ Try it Yourself

Create an IAM user with least privilege.

Enable MFA for the user.

Create a KMS key and encrypt an S3 bucket.

Enable GuardDuty and review findings.

Enable Security Hub and check compliance status.

Deploy WAF with a rule blocking SQL injection.