# 📺 AWS Course on YouTube  
🎥 [Watch the full course here](https://youtu.be/R6yysJg_rKE?list=PLJB9b1bbB85EabGxfihssYhe46dZRHXfn)

---

# 🗄️ AWS Databases – RDS, Aurora & DynamoDB

AWS offers both **Relational** and **NoSQL** database services.  
As a **Cloud Engineer**, you’ll often work with **RDS (Relational Database Service)**, **Aurora**, and **DynamoDB**.

---

## 📌 RDS (Relational Database Service)

Amazon RDS manages **relational databases** (MySQL, PostgreSQL, MariaDB, Oracle, SQL Server).  
AWS handles **patching, backups, replication, scaling, and HA**.

### Key Features
- Supports multiple engines (MySQL, PostgreSQL, etc.)  
- Automated backups and snapshots  
- Multi-AZ for high availability  
- Read Replicas for scaling  
- Encryption at rest and in transit  

---

### 🚀 Create RDS Instance – Console

1. Go to **AWS Console → RDS → Databases → Create database**.  
2. Choose **Standard Create**.  
3. Select engine (e.g., **MySQL**).  
4. Template → Free tier.  
5. Settings:
   - DB Identifier: `mydb`  
   - Master username: `admin`  
   - Password: `mypassword`  
6. Instance size → `db.t3.micro` (Free tier).  
7. Storage → 20 GB GP2 (default).  
8. Networking:
   - VPC → Default/custom.  
   - Public access → Yes (for testing).  
   - Security Group → Allow port `3306`.  
9. Enable **Automatic Backups**.  
10. Click **Create Database**.  

## ✅ Connect from EC2:  

mysql -h <endpoint> -P 3306 -u admin -p

## 🚀 Create RDS Instance – CLI
aws rds create-db-instance \
  --db-instance-identifier mydb \
  --db-instance-class db.t3.micro \
  --engine mysql \
  --master-username admin \
  --master-user-password mypassword123 \
  --allocated-storage 20


Check status:

aws rds describe-db-instances --db-instance-identifier mydb


Delete DB:

aws rds delete-db-instance --db-instance-identifier mydb --skip-final-snapshot

## 📌 Amazon Aurora

Aurora is an AWS-managed relational database, compatible with MySQL and PostgreSQL, but 5x faster.

Features

Serverless Aurora → Auto-scale compute based on load.

Multi-AZ & replication built-in.

Highly durable storage.

Ideal for cloud-native, high-performance apps.

Console Steps

Go to RDS → Create Database.

Choose Amazon Aurora.

Select Aurora MySQL or Aurora PostgreSQL.

Choose Serverless v2 (scales automatically).

Configure cluster identifier, credentials, and networking.

Create Aurora cluster → endpoint provided.

## 📌 DynamoDB (NoSQL)

Amazon DynamoDB is a fully managed NoSQL database.
Great for serverless apps, IoT, gaming, and high-scale APIs.

Features

Key-value & document-based

Fully managed, auto-scaling

Single-digit millisecond latency

Global tables (multi-region replication)

Integrated with Lambda for serverless

## 🚀 Create DynamoDB Table – Console

Go to DynamoDB → Tables → Create Table.

Table name: Users

Partition Key: UserID (String)

Sort Key (optional): Email (String)

Capacity mode:

On-demand (auto-scale)

Provisioned (manual RCU/WCU)

Create table.

Insert data:

Go to Explore Table → Items → Create Item.

Add JSON:

{
  "UserID": "101",
  "Name": "Sufyan",
  "Role": "DevOps Engineer"
}

## 🚀 Create DynamoDB Table – CLI
aws dynamodb create-table \
  --table-name Users \
  --attribute-definitions AttributeName=UserID,AttributeType=S \
  --key-schema AttributeName=UserID,KeyType=HASH \
  --billing-mode PAY_PER_REQUEST


Insert item:

aws dynamodb put-item \
  --table-name Users \
  --item '{"UserID": {"S": "101"}, "Name": {"S": "Sufyan"}, "Role": {"S": "DevOps Engineer"}}'


Query:

aws dynamodb get-item --table-name Users --key '{"UserID": {"S": "101"}}'

## ✅ Try it Yourself

Create an RDS MySQL instance and connect using EC2.

Enable Multi-AZ deployment for HA.

Create a Read Replica and test queries.

Launch an Aurora Serverless cluster.

Create a DynamoDB table, insert items, and query using CLI.