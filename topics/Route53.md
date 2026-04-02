# 🌍 AWS Route 53 Complete Setup Guide

---

## 🔹 What is Route 53?

Amazon Route 53 is a **DNS (Domain Name System)** service that:

- Converts domain names → IP addresses  
- Routes traffic to AWS resources  
- Performs health checks  
- Provides high availability  

---

## 🔹 Prerequisites

Before setting up Route 53, ensure:

- AWS Account  
- A domain name (Route 53 / GoDaddy / Namecheap)  
- A target resource:
  - EC2 instance
  - Application Load Balancer (ALB)
  - S3 static website
  - CloudFront distribution

---

## 🔹 Step 1: Create Hosted Zone

1. Go to **AWS Console → Route 53**
2. Click **Hosted Zones**
3. Click **Create Hosted Zone**
4. Enter:
   - Domain Name: `example.com`
   - Type: Public Hosted Zone
5. Click **Create**

---

## 🔹 Step 2: Note Name Servers (NS Records)

After creating hosted zone, you will see:

- NS (Name Server) records
- SOA record

Example:

ns-123.awsdns-45.com  
ns-456.awsdns-67.net  

---

## 🔹 Step 3: Update Domain Registrar

If domain is from **GoDaddy / Namecheap**:

1. Go to your domain registrar
2. Open **DNS / Nameserver settings**
3. Replace existing nameservers with Route 53 NS records
4. Save changes

⏱️ DNS propagation may take a few minutes to hours

---

## 🔹 Step 4: Create DNS Record

Go to Hosted Zone → Click **Create Record**

---

### 👉 A Record (For EC2 / ALB)

- Record Type: A
- Value: Public IP or ALB DNS
- Routing: Simple

Example:

example.com → 3.110.xx.xx

---

### 👉 Alias Record (Recommended for AWS)

- Use for ALB / CloudFront / S3
- No need to enter IP
- Select AWS resource directly

Example:

example.com → ALB

---

### 👉 CNAME Record

- Used for subdomains

Example:

www.example.com → example.com

---

## 🔹 Step 5: Test DNS

Run:

```bash
nslookup example.com
````

Or open browser:

[https://example.com](https://example.com)

---

## 🔹 Routing Policies (Important)

### 1️⃣ Simple Routing

* Default routing
* One resource

---

### 2️⃣ Weighted Routing

* Distribute traffic by percentage

Example:

* 70% → Server A
* 30% → Server B

---

### 3️⃣ Latency Routing

* Route to nearest region

---

### 4️⃣ Failover Routing

* Primary + Backup

If primary fails → switch to backup

---

### 5️⃣ Geolocation Routing

* Route based on user location

---

## 🔹 Health Checks

* Monitor application health
* Automatically reroute traffic if unhealthy

Example:

* EC2 down → Route to another instance

---

## 🔹 TTL (Time To Live)

* Defines DNS cache time

Example:

* 300 seconds (5 min)

Lower TTL → Faster updates
Higher TTL → Better performance

---

## 🔹 Common Use Cases

* Hosting websites
* Connecting domain to EC2 / ALB
* Global traffic routing
* Failover setup
* Load balancing

---

## 🔹 Best Practices

* Use **Alias records** for AWS resources
* Enable **health checks**
* Use **multiple AZs**
* Set proper TTL values
* Use **CloudFront for global apps**

---

## 🔹 Common Issues

| Issue                | Cause                 |
| -------------------- | --------------------- |
| Domain not resolving | NS not updated        |
| Slow DNS             | High TTL              |
| Wrong routing        | Incorrect record type |
| Delay in changes     | DNS propagation       |

---

## 🔹 Quick Summary

* Route 53 = DNS service
* Hosted Zone = Domain container
* Record = Mapping (Domain → Resource)
* NS = Connect domain to Route 53

---

## 🔹 One-Line Summary

👉 Route 53 connects your domain name to your AWS resources.
