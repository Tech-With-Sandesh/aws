# 🚀 AWS Debugging Guide: EC2 in Public Subnet but SSH Not Working

## 📌 Overview

A very common real-world issue:

> EC2 instance is running in a public subnet with a public IP, but SSH (port 22) is not working.

This is almost always a **networking or security configuration issue**.

---

## 🧠 What Should Work (Expected Flow)

```
Your Laptop → Internet → Internet Gateway → Route Table → Subnet → EC2 → SSH (port 22)
```

If SSH fails, the issue is somewhere in this path.

---

## 🎯 Goal

Identify:
- Where the connection is getting blocked
- Fix access to EC2 instance

---

## 🔍 Step 1: Verify Public IP

### Check:
- Instance has a **public IP or Elastic IP**

```bash
aws ec2 describe-instances
```

### Problem:
- No public IP → cannot access from internet

### Fix:
- Enable auto-assign public IP
- Or attach Elastic IP

---

## 🔍 Step 2: Check Security Group (MOST COMMON)

### Problem:
Security group does not allow SSH

### Verify:
- Inbound rule:
```
Type: SSH
Port: 22
Source: Your IP (or 0.0.0.0/0 for testing)
```

### Fix:
Add rule:
```
Port 22 → allow from your IP
```

---

## 🔍 Step 3: Check Network ACL (NACL)

### Problem:
NACL blocks traffic

### Check:
- Inbound rule allows port 22
- Outbound rule allows ephemeral ports (1024–65535)

### Fix:
Allow:
```
Inbound: 22
Outbound: 1024-65535
```

---

## 🔍 Step 4: Check Route Table

### Problem:
No route to Internet Gateway

### Check:
```
0.0.0.0/0 → igw-xxxx
```

### Fix:
Attach Internet Gateway and update route table

---

## 🔍 Step 5: Verify Subnet is Public

### Condition for public subnet:
- Route to Internet Gateway exists

If not → subnet is effectively private

---

## 🔍 Step 6: Check Instance OS Firewall

### Problem:
OS-level firewall blocks SSH

### Check:
```bash
sudo ufw status
sudo iptables -L
```

### Fix:
Allow port 22

---

## 🔍 Step 7: Check SSH Service

### Problem:
SSH service not running

### Check:
```bash
sudo systemctl status sshd
```

### Fix:
```bash
sudo systemctl start sshd
```

---

## 🔍 Step 8: Check Key Pair

### Problem:
- Wrong private key
- Incorrect permissions

### Fix:
```bash
chmod 400 key.pem
ssh -i key.pem ec2-user@<public-ip>
```

---

## 🔍 Step 9: Check Instance State

### Problem:
Instance not fully initialized

### Fix:
- Wait for status checks to pass

---

## 🔍 Step 10: Corporate Network / Firewall

### Problem:
Your local network blocks port 22

### Fix:
- Try from different network
- Use VPN or hotspot

---

## 📊 Common Root Causes

| Issue | Impact |
|------|--------|
| Security Group missing SSH | No access |
| No public IP | Not reachable |
| Route table missing IGW | No internet |
| NACL blocking | Traffic dropped |
| OS firewall | SSH blocked |
| SSH service down | Connection refused |

---

## 🎯 Final Debugging Checklist

- [ ] Public IP attached
- [ ] Security group allows port 22
- [ ] Route table has IGW
- [ ] NACL allows traffic
- [ ] SSH service running
- [ ] Correct key used
- [ ] No local firewall blocking

---

## 🚀 Best Practices

- Restrict SSH to your IP (not 0.0.0.0/0)
- Use Bastion Host instead of direct SSH
- Use AWS SSM Session Manager (no SSH needed)
- Monitor access logs

---

## 📌 Conclusion

If EC2 in a public subnet is not reachable via SSH,  
the issue is almost always:

- Security group  
- Route table  
- Missing public IP  

Systematic debugging helps identify the exact cause quickly 🚀

---

## ⭐ Support

- Star ⭐ the repo  
- Share with others  
- Follow for more DevOps content  
