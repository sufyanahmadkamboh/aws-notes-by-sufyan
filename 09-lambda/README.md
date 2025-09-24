# 📺 AWS Course on YouTube  
🎥 [Watch the full course here](https://youtu.be/R6yysJg_rKE?list=PLJB9b1bbB85EabGxfihssYhe46dZRHXfn)

---

# ⚡ AWS Lambda (Serverless Functions)

AWS **Lambda** lets you run code **without managing servers**.  
You only pay for the compute time you consume.  
Lambda scales automatically and integrates with 200+ AWS services.

---

## 📌 Key Features

- Fully managed compute (no servers needed).  
- Supports multiple runtimes: Node.js, Python, Java, Go, .NET, Ruby.  
- **Triggers**: S3, DynamoDB, API Gateway, CloudWatch, SNS, SQS, EventBridge.  
- **Layers**: Reusable libraries for functions.  
- **IAM Role**: Functions need a role with permissions.  
- Scales automatically based on events.  

---

## 📌 Use Cases

- Serverless APIs (via API Gateway + Lambda).  
- Event-driven tasks (e.g., S3 upload → process file).  
- Real-time data processing.  
- Automation scripts.  
- Scheduled jobs (via CloudWatch Events).  

---

## 🚀 Create a Lambda Function – Console

1. Open **AWS Console → Lambda → Create Function**.  
2. Choose:
   - **Author from scratch**  
   - Function name: `HelloLambda`  
   - Runtime: **Python 3.9** (or Node.js, etc.)  
   - Permissions: Create new role with basic Lambda permissions.  
3. Click **Create Function**.  
4. In **Code editor**, paste:  

   ```python
   def lambda_handler(event, context):
       return {
           'statusCode': 200,
           'body': 'Hello from Sufyan Lambda!'
       }

Deploy → Test function with sample event.

## 🚀 Create a Lambda Function – CLI
1️⃣ Create IAM Role for Lambda
aws iam create-role \
  --role-name lambda-ex-role \
  --assume-role-policy-document file://trust-policy.json


Example trust-policy.json:

{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": { "Service": "lambda.amazonaws.com" },
      "Action": "sts:AssumeRole"
    }
  ]
}


Attach basic execution policy:

aws iam attach-role-policy \
  --role-name lambda-ex-role \
  --policy-arn arn:aws:iam::aws:policy/service-role/AWSLambdaBasicExecutionRole

2️⃣ Create Deployment Package
echo "def lambda_handler(event, context): return {'statusCode':200,'body':'Hello from CLI Lambda!'}" > lambda_function.py
zip function.zip lambda_function.py

3️⃣ Create Lambda Function
aws lambda create-function \
  --function-name HelloLambda \
  --runtime python3.9 \
  --role arn:aws:iam::<account-id>:role/lambda-ex-role \
  --handler lambda_function.lambda_handler \
  --zip-file fileb://function.zip

4️⃣ Invoke Function
aws lambda invoke --function-name HelloLambda response.json
cat response.json

## 📌 Lambda Triggers

S3 Trigger: Process file when uploaded.

DynamoDB Stream: React to DB changes.

API Gateway: Expose REST/HTTP API.

CloudWatch Events: Run scheduled jobs.

SNS/SQS: Consume messages.

Example: Attach trigger via Console → Lambda → Add Trigger → Select S3 bucket.

## 📌 Lambda Layers

Used to package and share libraries across functions.

Example: Put Python requests library in a layer → attach to multiple functions.

Console: Lambda → Layers → Create Layer → Upload .zip with dependencies.

## 📌 Monitoring

CloudWatch Logs: Logs from Lambda functions.

CloudWatch Metrics: Invocations, Duration, Errors, Throttles.

X-Ray: Distributed tracing for debugging.

## ✅ Try it Yourself

Create a Lambda that returns "Hello AWS!".

Add an S3 trigger → Upload a file → Lambda should log event info.

Create an API Gateway → Lambda integration → Access Lambda via HTTP.

Add a CloudWatch Events trigger → Schedule Lambda every 5 minutes.
