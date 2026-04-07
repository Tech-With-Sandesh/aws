# 🔐 AWS IAM User vs IAM Role (Real Examples)

---

## 🎯 Overview

This document explains real-world usage of:

- IAM User  
- IAM Role  

---

## 👤 IAM User Example

### 📌 Use Case: Developer Access

A developer needs to:

- Login to AWS Console  
- Create EC2 instances  
- Access S3  

---

### 🔹 IAM User Setup

```json
{
  "User": "dev-user",
  "Permissions": [
    "AmazonEC2FullAccess",
    "AmazonS3ReadOnlyAccess"
  ]
}
```

---

### 🔹 Access Method

- AWS Console login (username + password)  
- Access Keys (for CLI)

```bash
aws configure
```

---

### 🔹 Key Points

- Long-term credentials  
- Used by humans or applications  
- Requires secure handling  

---

## 🧑‍💻 IAM Role Example (EC2 → S3)

### 📌 Use Case: EC2 accessing S3

Instead of storing access keys inside EC2,  
we attach an IAM Role.

---

### 🔹 IAM Role Policy

```json
{
  "Role": "EC2-S3-Access",
  "Permissions": [
    "AmazonS3ReadOnlyAccess"
  ]
}
```

---

### 🔹 Attach Role to EC2

- Go to EC2 Console  
- Select instance  
- Attach IAM Role: `EC2-S3-Access`  

---

### 🔹 Access S3 from EC2

```bash
aws s3 ls
```

👉 No credentials required inside the instance  

---

### 🔹 Key Points

- No hardcoded credentials  
- Uses temporary credentials (STS)  
- More secure than IAM User  

---

## ⚡ IAM Role Example (Lambda → DynamoDB)

### 📌 Use Case: Lambda writing to DynamoDB

---

### 🔹 IAM Role Policy

```json
{
  "Role": "Lambda-DynamoDB-Access",
  "Permissions": [
    "AmazonDynamoDBFullAccess"
  ]
}
```

---

### 🔹 Flow

Lambda → IAM Role → DynamoDB  

---

## 🔑 Key Differences

| Feature | IAM User | IAM Role |
|--------|----------|----------|
| Identity | Fixed | Assumed |
| Credentials | Long-term | Temporary |
| Usage | Humans / Apps | AWS Services |
| Security | Lower | Higher |

---

## 🧠 Best Practices

- Avoid using IAM Users for applications  
- Use IAM Roles for AWS services  
- Follow least privilege principle  
- Rotate credentials regularly  

---

## 🎯 One-Line Summary

👉 IAM User = Direct access with credentials  
👉 IAM Role = Temporary access without storing credentials  

---
