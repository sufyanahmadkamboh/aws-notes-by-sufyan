# 📺 AWS Course on YouTube  
🎥 [Watch the full course here](https://youtu.be/R6yysJg_rKE?list=PLJB9b1bbB85EabGxfihssYhe46dZRHXfn)

---

# 🌍 Amazon Route 53 (DNS & Traffic Management)

Amazon **Route 53** is a scalable and highly available **DNS service**.  
It lets you manage **domains, DNS records, and routing policies** for applications worldwide.

---

## 📌 Key Features

- **Domain Registration** → Buy/manage domains.  
- **DNS Service** → Map names (e.g., `example.com`) to IPs/ALBs.  
- **Health Checks & Failover** → Detect unhealthy endpoints.  
- **Traffic Routing Policies**:  
  - Simple  
  - Weighted  
  - Latency-based  
  - Failover  
  - Geolocation  

---

## 📌 Hosted Zones

- **Public Hosted Zone** → Manages DNS for a domain accessible over the internet.  
- **Private Hosted Zone** → DNS resolution inside a VPC only.  

---

## 📌 Common Record Types

| Record Type | Example                  | Use Case                         |
|-------------|--------------------------|----------------------------------|
| A           | `example.com → 192.0.2.1` | IPv4 address                     |
| AAAA        | `example.com → ::1`       | IPv6 address                     |
| CNAME       | `www.example.com → example.com` | Alias for another domain  |
| MX          | `example.com → mail server` | Email routing                 |
| TXT         | `"v=spf1 include:aws"`    | Verification / security records  |
| Alias       | `example.com → ALB / S3`  | AWS resources (no extra cost)    |

---

## 📌 Creating Hosted Zone & Records – Console

### 1️⃣ Create a Hosted Zone
1. Go to **Route 53 → Hosted Zones → Create Hosted Zone**.  
2. Enter domain name (must be registered).  
3. Choose:
   - Public Hosted Zone (internet-facing)  
   - Private Hosted Zone (inside VPC)  

AWS provides **NS (Name Server)** records → update them in your domain registrar.  

---

### 2️⃣ Add Records
Example: Point `www.example.com` to an ALB.

1. Go to **Hosted Zone → Create Record**.  
2. Name: `www`.  
3. Type: Alias → Application Load Balancer.  
4. Save.  

Example: TXT record for email verification (e.g., Google Workspace):  
- Name: `@`  
- Type: TXT  
- Value: `"v=spf1 include:_spf.google.com ~all"`  

---

## 📌 Creating Hosted Zone & Records – CLI

### 1️⃣ Create Hosted Zone

aws route53 create-hosted-zone \
  --name example.com \
  --caller-reference 123456789

2️⃣ Create Record Set
aws route53 change-resource-record-sets \
  --hosted-zone-id Z123456ABCDEF \
  --change-batch '{
    "Changes": [{
      "Action": "CREATE",
      "ResourceRecordSet": {
        "Name": "www.example.com",
        "Type": "A",
        "AliasTarget": {
          "HostedZoneId": "Z2P70J7EXAMPLE", 
          "DNSName": "my-alb-123456.elb.amazonaws.com",
          "EvaluateTargetHealth": false
        }
      }
    }]
  }'

3️⃣ List Records
aws route53 list-resource-record-sets --hosted-zone-id Z123456ABCDEF

## 📌 Routing Policies

Simple Routing → Single record, e.g., example.com → 1 IP.

Weighted Routing → Split traffic, e.g., 70% → server A, 30% → server B.

Latency-based Routing → Route to closest region for lowest latency.

Failover Routing → Primary site + secondary site (disaster recovery).

Geolocation Routing → Route based on user’s geographic location.

Console:

Hosted Zone → Create Record → Select Routing Policy.

## 📌 Health Checks

Route 53 can monitor endpoints (HTTP, HTTPS, TCP).

If endpoint fails, traffic is routed to healthy targets (failover).

Console:

Route 53 → Health Checks → Create Health Check.

Set endpoint & failure threshold.

CLI:

aws route53 create-health-check \
  --caller-reference "check123" \
  --health-check-config '{
    "IPAddress": "192.0.2.1",
    "Port": 80,
    "Type": "HTTP",
    "ResourcePath": "/"
  }'

## ✅ Try it Yourself

Register a domain in Route 53 (or use an existing one).

Create a Public Hosted Zone.

Add an A record pointing www.<yourdomain> → EC2 or ALB.

Create a TXT record for domain verification.

Configure a failover policy:

Primary: EC2 in region A.

Secondary: EC2 in region B.

Test DNS resolution with:

nslookup www.example.com
dig www.example.com