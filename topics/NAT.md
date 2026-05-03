# 🚀 AWS Networking Debugging Guide: Private Subnet + NAT Gateway, But No Internet Access

## 📌 Overview

A common real-world issue:

> An EC2 instance is running in a private subnet.  
> The route table has a default route (0.0.0.0/0) pointing to a NAT Gateway,  
> but the instance still cannot access the internet.

This indicates a **misconfiguration in the networking path between EC2 → NAT → Internet Gateway**.

---

## 🧠 Expected Network Flow

```
EC2 (Private Subnet)
        ↓
Route Table (0.0.0.0/0 → NAT Gateway)
        ↓
NAT Gateway (in Public Subnet)
        ↓
Internet Gateway
        ↓
Internet
```

If any component in this chain is misconfigured → no internet access.

---

## 🎯 Goal

Identify:
- Where the traffic is blocked
- Why NAT is not forwarding traffic
- Fix the root cause

---

## 🔍 Step 1: Verify NAT Gateway Placement (MOST COMMON ISSUE)

### Requirement:
NAT Gateway **must be in a public subnet**

### Public subnet must have:
```
0.0.0.0/0 → Internet Gateway (IGW)
```

### Problem:
- NAT placed in private subnet → no internet

### Fix:
- Move NAT Gateway to public subnet
- Ensure public subnet has IGW route

---

## 🔍 Step 2: Check Elastic IP (EIP) on NAT Gateway

### Requirement:
NAT Gateway must have an **Elastic IP**

### Problem:
- NAT without EIP → cannot reach internet

### Fix:
- Attach Elastic IP to NAT Gateway

---

## 🔍 Step 3: Verify Route Table of Private Subnet

### Must have:
```
0.0.0.0/0 → NAT Gateway
```

### Problem:
- Route missing or pointing incorrectly

### Fix:
- Update route table correctly

---

## 🔍 Step 4: Check Route Table of Public Subnet (NAT Subnet)

### Must have:
```
0.0.0.0/0 → Internet Gateway
```

### Problem:
- No IGW route → NAT cannot forward traffic

---

## 🔍 Step 5: Check Internet Gateway Attachment

### Problem:
- IGW not attached to VPC

### Fix:
- Attach Internet Gateway to VPC

---

## 🔍 Step 6: Check Security Groups

### Important:
Security groups are **stateful**

### Requirement:
- Outbound traffic allowed (default is allow)

### Problem:
- Outbound blocked

---

## 🔍 Step 7: Check Network ACLs (NACL)

### Requirement:
Allow:
- Outbound: 0.0.0.0/0
- Inbound: ephemeral ports (1024–65535)

### Problem:
- NACL blocking return traffic

---

## 🔍 Step 8: Check DNS Resolution

### Problem:
Instance cannot resolve domain names

### Debug:
```bash
nslookup google.com
```

### Fix:
- Enable DNS in VPC settings

---

## 🔍 Step 9: Check Instance Configuration

### Problem:
- No proper route inside OS
- Firewall blocking

### Debug:
```bash
ping 8.8.8.8
curl google.com
```

---

## 🔍 Step 10: Check NAT Gateway Status

### Problem:
- NAT Gateway is not in "Available" state

### Fix:
- Wait for provisioning
- Recreate if failed

---

## 📊 Common Root Causes

| Issue | Impact |
|------|--------|
| NAT in private subnet | No internet |
| Missing Elastic IP | NAT fails |
| No IGW route | Traffic blocked |
| NACL blocking | No return traffic |
| DNS disabled | Domain resolution fails |
| Security group restriction | Outbound blocked |

---

## 🎯 Final Debugging Checklist

- [ ] NAT Gateway in public subnet  
- [ ] Elastic IP attached to NAT  
- [ ] Private subnet route → NAT  
- [ ] Public subnet route → IGW  
- [ ] IGW attached to VPC  
- [ ] Security groups allow outbound  
- [ ] NACL allows traffic  
- [ ] DNS working  
- [ ] NAT Gateway status is available  

---

## 🚀 Best Practices

- Always place NAT Gateway in public subnet  
- Use separate route tables for public/private subnets  
- Monitor NAT Gateway metrics (CloudWatch)  
- Use VPC flow logs for debugging  

---

## 📌 Conclusion

If a private subnet instance cannot access the internet despite NAT configuration:

👉 The issue is usually:
- NAT placement  
- Route table misconfiguration  
- Missing IGW or EIP  

Understanding the full network path is key to debugging 🚀

---

## ⭐ Support

- Star ⭐ the repo  
- Share with others  
- Follow for more DevOps content  

---
