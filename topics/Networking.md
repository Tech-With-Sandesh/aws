# ☁️ AWS Networking Guide (Complete)

---

## 🎯 Overview

This guide covers **core AWS networking concepts**:

- VPC  
- Subnet  
- Route Table  
- Internet Gateway (IGW)  
- NAT Gateway  
- Security Groups & NACL  
- Public vs Private Subnet  

---

## 🧠 What is VPC?

**VPC (Virtual Private Cloud)** is your private network in AWS.

👉 Like your own data center in the cloud

---

## 📦 VPC Example

```
VPC: 10.0.0.0/16
```

- Total IPs: 65,536  

---

## 🔹 Subnet

Subnet is a smaller network inside VPC.

---

### Example

```
Public Subnet: 10.0.1.0/24
Private Subnet: 10.0.2.0/24
```

---

## 🌐 Public vs Private Subnet

### Public Subnet

- Has route to Internet Gateway  
- Can access internet  

---

### Private Subnet

- No direct internet access  
- Uses NAT Gateway  

---

## 🔑 Key Rule

👉 Subnet becomes **public or private based on Route Table**

---

## 🧭 Route Table

Route table defines where traffic goes.

---

### Example Route

```
0.0.0.0/0 → igw-123456
```

👉 Send all traffic to internet  

---

## 🌍 Internet Gateway (IGW)

- Allows communication with internet  
- Attached to VPC  

---

### Flow

```
EC2 → Route Table → IGW → Internet
```

---

## 🔒 NAT Gateway

Used for **private subnet to access internet**

---

### Flow

```
Private EC2 → NAT Gateway → Internet
```

👉 No incoming traffic allowed  

---

## 🔐 Security Group vs NACL

### Security Group (Stateful)

- Works at instance level  
- Allows response automatically  

---

### NACL (Stateless)

- Works at subnet level  
- Needs rules for both directions  

---

## 🔄 Full Architecture

```
Internet
   ↓
Internet Gateway
   ↓
Public Subnet (Web Server)
   ↓
Private Subnet (App/DB)
   ↓
NAT Gateway (for outbound)
```

---

## ⚡ Key Concepts Summary

| Component | Purpose |
|----------|--------|
| VPC | Network |
| Subnet | Smaller network |
| Route Table | Traffic direction |
| IGW | Internet access |
| NAT | Private → Internet |
| Security Group | Instance firewall |
| NACL | Subnet firewall |

---

## 🧠 Memory Trick

- VPC = Network  
- Subnet = Section  
- Route Table = Directions  
- IGW = Internet access  
- NAT = Outbound internet  

---

## 🎯 One-Line Summary

👉 AWS networking is all about controlling **who can talk to whom and how traffic flows**

---
