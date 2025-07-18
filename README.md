# 🚀 AWS CI/CD Full-Stack App Deployment

Deploy a production-grade full-stack app using AWS CI/CD services — React frontend, Node.js backend, MySQL database, and automate everything using CodePipeline, CodeBuild, CodeDeploy, and EC2.

---

## 📁 Project Structure

aws-cicd-full-stack-app/
├── frontend/ # React App + buildspec.yml
└── backend/ # Node.js API + MySQL
├── app.js
├── appspec.yml
├── package.json
└── scripts/
├── install_dependencies.sh
├── start_server.sh
└── stop_server.sh

---

## ✅ CI/CD Workflow Summary

- GitHub → CodePipeline → CodeBuild (React) → S3 (static site)
- GitHub → CodePipeline → CodeDeploy (Node.js) → EC2 (backend)
- MySQL is installed manually on the backend EC2 instance.

---

## 🧱 Step-by-Step Deployment Guide

### ✅ STEP 1: Create GitHub Repository

- Create a GitHub repo: `aws-cicd-full-stack-app`
- Clone it to your local system:
  ```bash
  git clone https://github.com/yourusername/aws-cicd-full-stack-app.git
✅ STEP 2: Setup Frontend (React)
cd frontend
npx create-react-app .
Customize your app as needed.

Add frontend/buildspec.yml to define CodeBuild steps.

✅ STEP 3: Setup Backend (Node.js)

cd backend
npm init -y
npm install express mysql
Create app.js (Node.js API)

Create scripts/ folder with:

install_dependencies.sh

start_server.sh

stop_server.sh

Add appspec.yml for CodeDeploy.

✅ STEP 4: Create IAM Roles in AWS
EC2 Role: EC2CodeDeployRole → Policy: AmazonEC2RoleforAWSCodeDeploy

CodeDeploy Role: CodeDeployServiceRole → Policy: AWSCodeDeployRole

CodePipeline Role: CodePipelineServiceRole → Policy: AWSCodePipelineFullAccess

✅ STEP 5: Launch EC2 for Backend
Amazon Linux 2 → t2.micro

Open ports: 22 (SSH), 3000 (App), 3306 (MySQL)

Attach EC2CodeDeployRole

Advanced settings → Add CodeDeploy agent installation script:


#!/bin/bash
yum update -y
yum install ruby wget -y
cd /home/ec2-user
wget https://bucket-name/latest/install
chmod +x ./install
./install auto
systemctl start codedeploy-agent

✅ STEP 6: Install & Configure MySQL

sudo yum install mysql-server -y
sudo systemctl start mysqld

Connect to MySQL:


CREATE DATABASE devdb;
CREATE USER 'devuser'@'%' IDENTIFIED BY 'password';
GRANT ALL PRIVILEGES ON devdb.* TO 'devuser'@'%';

✅ STEP 7: Upload Backend ZIP to S3

cd backend
zip -r backend.zip .
Upload backend.zip to an S3 bucket.

✅ STEP 8: Create CodeDeploy App
Application name: backend-deploy-app

Deployment group: Linked to EC2 using tags

Attach role: CodeDeployServiceRole

Enable load balancing if required.

✅ STEP 9: Create CodePipeline
Source: GitHub (this repo)

Build stage: CodeBuild project for frontend/

Deploy stage: CodeDeploy for backend

✅ STEP 10: Push Code to GitHub

git add .
git commit -m "Initial CI/CD project setup"
git push origin main




