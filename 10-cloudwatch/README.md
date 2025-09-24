# 📺 AWS Course on YouTube  
🎥 [Watch the full course here](https://youtu.be/R6yysJg_rKE?list=PLJB9b1bbB85EabGxfihssYhe46dZRHXfn)

---

# 📊 Amazon CloudWatch – Logs, Metrics & Alarms

Amazon **CloudWatch** provides **monitoring, logging, and observability** for AWS resources and applications.  
It helps track performance, detect issues, and trigger automated actions.

---

## 📌 Key Components

- **Metrics** → Numeric data (CPU, memory, requests/sec).  
- **Logs** → Application/system logs (from Lambda, EC2, etc.).  
- **Alarms** → Trigger actions when thresholds are breached.  
- **Dashboards** → Custom visualization panels.  
- **Events / EventBridge** → React to system events (automation).  

---

## 📌 Metrics

AWS services automatically send metrics to CloudWatch.

### Examples:
- EC2: `CPUUtilization`, `NetworkIn`, `DiskReadOps`  
- RDS: `FreeStorageSpace`, `DatabaseConnections`  
- Lambda: `Invocations`, `Errors`, `Duration`  

---

### 🚀 Create Alarm – Console

1. Go to **CloudWatch → Alarms → Create Alarm**.  
2. Select a metric (e.g., EC2 → Per-Instance Metrics → `CPUUtilization`).  
3. Set threshold (e.g., Alarm if CPU > 70% for 5 min).  
4. Choose action:
   - Notify via **SNS topic** (email, SMS).  
   - Trigger **Auto Scaling policy**.  
5. Name alarm → Create.  

---

### 🚀 Create Alarm – CLI


aws cloudwatch put-metric-alarm \
  --alarm-name HighCPU \
  --metric-name CPUUtilization \
  --namespace AWS/EC2 \
  --statistic Average \
  --period 300 \
  --threshold 70 \
  --comparison-operator GreaterThanThreshold \
  --dimensions Name=InstanceId,Value=i-0123456789abcdef0 \
  --evaluation-periods 2 \
  --alarm-actions arn:aws:sns:us-east-1:123456789012:MySNSTopic

## 📌 Logs

Applications send logs to CloudWatch Log Groups.

Each log group contains log streams (e.g., per EC2, per Lambda).

Console

Go to CloudWatch → Logs → Log Groups.

Select group → View log streams.

Search logs, filter by keywords.

CLI
# List log groups
aws logs describe-log-groups

# View recent logs
aws logs get-log-events \
  --log-group-name /aws/lambda/HelloLambda \
  --log-stream-name 2025/09/24/[$LATEST]abcd1234

## 📌 Dashboards

Dashboards let you visualize metrics in custom panels.

Console

Go to CloudWatch → Dashboards → Create Dashboard.

Add widgets (line/number/pie).

Choose metrics (e.g., EC2 CPU, S3 requests).

CLI
aws cloudwatch put-dashboard \
  --dashboard-name MyDashboard \
  --dashboard-body '{"widgets":[{"type":"metric","x":0,"y":0,"width":6,"height":6,
  "properties":{"metrics":[["AWS/EC2","CPUUtilization","InstanceId","i-0123456789abcdef0"]],
  "period":300,"stat":"Average","region":"us-east-1"}}]}'

## 📌 Events / EventBridge

CloudWatch Events (EventBridge) can trigger automation when events happen.

Example: Stop EC2 at night.

Console

Go to EventBridge → Rules → Create Rule.

Event Source → Schedule (cron/interval).

Target → EC2 → StopInstances API call.

CLI
aws events put-rule \
  --name StopEC2Rule \
  --schedule-expression "cron(0 22 * * ? *)"

aws events put-targets \
  --rule StopEC2Rule \
  --targets "Id"="1","Arn"="arn:aws:ec2:us-east-1:123456789012:instance/i-0123456789abcdef0"

## ✅ Try it Yourself

Launch an EC2 instance.

Create an alarm: CPUUtilization > 70% for 5 min → send email via SNS.

Enable logging in Lambda → view logs in CloudWatch.

Create a custom dashboard showing EC2 CPU + RDS connections.

Create EventBridge rule to stop EC2 at 10 PM daily.