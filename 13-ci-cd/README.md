# 📺 AWS Course on YouTube  
🎥 [Watch the full course here](https://youtu.be/rUTuNee9SBA?list=PLJB9b1bbB85F23di_ydm_cZ9efYqfRzTq)

---

# 🚀 CI/CD on AWS – CodeCommit, CodeBuild, CodeDeploy, CodePipeline

AWS provides a suite of **Developer Tools** to implement CI/CD pipelines:  

- **CodeCommit** → Git-based source control.  
- **CodeBuild** → Build & test code in isolated environments.  
- **CodeDeploy** → Deploy apps to EC2, Lambda, ECS.  
- **CodePipeline** → Orchestrate the CI/CD workflow.  

---

## 📌 CodeCommit (Git Repository)

### Console
1. Open **CodeCommit → Repositories → Create repository**.  
2. Name: `my-repo`.  
3. Clone repo locally:

   git clone https://git-codecommit.us-east-1.amazonaws.com/v1/repos/my-repo

CLI
aws codecommit create-repository --repository-name my-repo --repository-description "Demo repo"

## 📌 CodeBuild (Build Automation)
Console

Go to CodeBuild → Create Build Project.

Source → CodeCommit repo.

Environment → Managed image (Ubuntu, Node.js, Python, etc.).

Buildspec → Provide buildspec.yml or inline.

Artifacts → Store in S3.

Example buildspec.yml:

version: 0.2
phases:
  install:
    commands:
      - echo "Installing dependencies"
      - npm install
  build:
    commands:
      - echo "Building project"
      - npm run build
artifacts:
  files:
    - '**/*'

CLI
aws codebuild create-project \
  --name my-build \
  --source type=CODECOMMIT,location=https://git-codecommit.us-east-1.amazonaws.com/v1/repos/my-repo \
  --artifacts type=S3,location=my-build-artifacts-bucket \
  --environment type=LINUX_CONTAINER,image=aws/codebuild/standard:5.0,computeType=BUILD_GENERAL1_SMALL

## 📌 CodeDeploy (Application Deployment)

Supports deployments to:

EC2

Lambda

ECS

Console (EC2 Example)

Go to CodeDeploy → Applications → Create Application.

Compute platform: EC2/On-Premises.

Create a Deployment Group.

Choose EC2 instances with specific tags.

Attach IAM Role with CodeDeploy permissions.

Upload AppSpec file to repo:

Example appspec.yml:

version: 0.0
os: linux
files:
  - source: /
    destination: /var/www/html
hooks:
  AfterInstall:
    - location: scripts/restart.sh
      runas: root


Trigger deployment.

CLI
aws deploy create-application --application-name myapp --compute-platform Server

## 📌 CodePipeline (CI/CD Orchestration)

CodePipeline integrates source → build → deploy.

Console

Go to CodePipeline → Create Pipeline.

Pipeline name: MyPipeline.

Source: CodeCommit repo.

Build: CodeBuild project.

Deploy: CodeDeploy application.

Save → AWS creates a fully automated pipeline.

CLI
aws codepipeline create-pipeline --cli-input-json file://pipeline.json


Example pipeline.json:

{
  "pipeline": {
    "name": "MyPipeline",
    "roleArn": "arn:aws:iam::123456789012:role/AWS-CodePipeline-Service",
    "stages": [
      {
        "name": "Source",
        "actions": [{
          "name": "Source",
          "actionTypeId": {
            "category": "Source",
            "owner": "AWS",
            "provider": "CodeCommit",
            "version": "1"
          },
          "configuration": {
            "RepositoryName": "my-repo",
            "BranchName": "main"
          },
          "outputArtifacts": [{"name": "SourceOutput"}]
        }]
      },
      {
        "name": "Build",
        "actions": [{
          "name": "Build",
          "actionTypeId": {
            "category": "Build",
            "owner": "AWS",
            "provider": "CodeBuild",
            "version": "1"
          },
          "configuration": {"ProjectName": "my-build"},
          "inputArtifacts": [{"name": "SourceOutput"}],
          "outputArtifacts": [{"name": "BuildOutput"}]
        }]
      }
    ],
    "artifactStore": {
      "type": "S3",
      "location": "my-pipeline-artifacts-bucket"
    }
  }
}

## ✅ Try it Yourself

Create a CodeCommit repo and push a sample Node.js app.

Create a CodeBuild project with buildspec.yml.

Create a CodeDeploy app → deploy to EC2.

Create a CodePipeline linking all three.

Push new code → pipeline automatically builds & deploys it.