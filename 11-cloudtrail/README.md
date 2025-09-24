# 📺 AWS Course on YouTube  
🎥 [Watch the full course here](https://youtu.be/R6yysJg_rKE?list=PLJB9b1bbB85EabGxfihssYhe46dZRHXfn)

---

# 🔍 AWS CloudTrail – Governance & Auditing

Amazon **CloudTrail** records all **API calls & account activity** across AWS.  
It helps with **auditing, compliance, and security investigations**.

---

## 📌 Key Features

- Captures **who did what, when, and from where**.  
- Records **Console actions, CLI commands, SDK/API calls**.  
- Logs stored in **S3, CloudWatch Logs, or EventBridge**.  
- Supports **multi-region trails**.  
- Can trigger **alerts** for suspicious activity.  

---

## 📌 Events in CloudTrail

1. **Management Events** → Control plane actions (e.g., `RunInstances`, `CreateBucket`).  
2. **Data Events** → S3 object-level actions, Lambda invoke.  
3. **Insight Events** → Detect unusual activity.  

---

## 🚀 Enable CloudTrail – Console

1. Open **CloudTrail → Trails → Create Trail**.  
2. Trail name: `MyTrail`.  
3. Storage:
   - Create or choose an S3 bucket.  
   - Enable log file SSE encryption (recommended).  
4. Select:
   - **Management Events** → All.  
   - **Data Events** → Enable for S3/Lambda if needed.  
   - **Insights Events** → Optional anomaly detection.  
5. Apply trail to **all regions** (best practice).  
6. Finish → CloudTrail starts logging.  

View events:  
- Go to **CloudTrail → Event history**.  
- Search by **event name, user, resource**.  

---

## 🚀 Enable CloudTrail – CLI

### 1️⃣ Create an S3 Bucket for Logs

aws s3api create-bucket --bucket sufyan-cloudtrail-logs --region us-east-1

2️⃣ Create the Trail
aws cloudtrail create-trail \
  --name MyTrail \
  --s3-bucket-name sufyan-cloudtrail-logs \
  --is-multi-region-trail

3️⃣ Start Logging
aws cloudtrail start-logging --name MyTrail

4️⃣ Lookup Events
aws cloudtrail lookup-events --max-results 5

## 📌 Example Event Record (JSON)
{
  "eventVersion": "1.08",
  "userIdentity": {
    "type": "IAMUser",
    "userName": "alice"
  },
  "eventTime": "2025-09-24T12:34:56Z",
  "eventSource": "ec2.amazonaws.com",
  "eventName": "StartInstances",
  "awsRegion": "us-east-1",
  "sourceIPAddress": "203.0.113.25",
  "requestParameters": {"instancesSet": ["i-0123456789abcdef0"]},
  "responseElements": null
}

## 📌 Integration with CloudWatch

Send CloudTrail logs to CloudWatch Logs for alerts.

In Console → CloudTrail → Configure CloudWatch Logs.

Create Log Group: /aws/cloudtrail/mytrail.

Create Metric Filter (e.g., detect DeleteBucket).

Create Alarm → Notify via SNS.

## ✅ Try it Yourself

Create a CloudTrail trail for all regions.

Launch an EC2 instance → check Event History for RunInstances.

Delete an S3 bucket → verify CloudTrail logs it.

Create a CloudWatch alarm for suspicious events (ConsoleLogin failures).