# 📺 AWS Course on YouTube  
🎥 [Watch the full course here](https://youtu.be/R6yysJg_rKE?list=PLJB9b1bbB85EabGxfihssYhe46dZRHXfn)

---

# 💰 AWS Cost Optimization

AWS offers flexibility, but costs can grow fast if not managed properly.  
As a **Cloud Engineer**, you must design architectures that are **scalable and cost-efficient**.

---

## 📌 Key Principles of Cost Optimization

1. **Right-size resources** → Avoid overprovisioning.  
2. **Use Auto Scaling** → Match demand, no idle resources.  
3. **Choose pricing models wisely**:  
   - On-Demand  
   - Reserved Instances (RI)  
   - Savings Plans  
   - Spot Instances  
4. **Leverage managed services** → Pay per use (Lambda, DynamoDB on-demand).  
5. **Turn off unused resources** → Stopped EC2s, idle load balancers, unused EBS.  
6. **Use monitoring & alerts** → Budgets, Cost Explorer, Trusted Advisor.  

---

## 📌 Tools for Cost Management

### 1️⃣ AWS Cost Explorer
- Visualize cost & usage over time.  
- Find expensive services.  

Console:
- Go to **Billing → Cost Explorer**.  
- Filter by service, region, linked accounts.  

### CLI

aws ce get-cost-and-usage \
  --time-period Start=2025-09-01,End=2025-09-30 \
  --granularity MONTHLY \
  --metrics "UnblendedCost"

### 2️⃣ AWS Budgets

Set custom spending limits.

Get alerts via SNS when budget is exceeded.

Console:

Go to Billing → Budgets → Create Budget.

Budget type: Cost Budget.

Example: Limit = $50/month.

Add SNS topic → get email alerts.

CLI:

aws budgets create-budget \
  --account-id 123456789012 \
  --budget '{
    "BudgetName":"MonthlyBudget",
    "BudgetLimit":{"Amount":"50","Unit":"USD"},
    "TimeUnit":"MONTHLY",
    "BudgetType":"COST"
  }'

### 3️⃣ Trusted Advisor

Provides recommendations on cost, security, performance, fault tolerance.

Cost checks: Idle EC2, Underutilized RDS, Unassociated Elastic IPs.

Console:

Go to Trusted Advisor in Support Center.

Review Cost Optimization checks.

### 4️⃣ Spot & Reserved Pricing

Spot Instances → Up to 90% cheaper than On-Demand. Best for fault-tolerant, flexible workloads.

Reserved Instances / Savings Plans → Commit 1–3 years → Save up to 70%.

CLI Example (Spot request):

aws ec2 request-spot-instances \
  --instance-count 1 \
  --type "one-time" \
  --launch-specification file://spec.json

### 5️⃣ S3 Storage Classes & Lifecycle

Move infrequent data to S3 IA (Infrequent Access).

Archive to Glacier for lowest cost.

Automate with Lifecycle policies.

Console:

Go to S3 → Bucket → Management → Lifecycle rules.

## ✅ Try it Yourself

Open Cost Explorer → Check which service is most expensive.

Create a budget of $20/month and enable email alerts.

Enable Trusted Advisor → Check unused resources.

Launch an EC2 Spot Instance and compare cost with On-Demand.

Create an S3 Lifecycle Rule → Move objects to Glacier after 30 days.