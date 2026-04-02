# ⚖️ AWS ALB vs NLB Cheat Sheet

---

## 🔹 Overview

| Feature | ALB (Application Load Balancer) | NLB (Network Load Balancer) |
|--------|--------------------------------|------------------------------|
| OSI Layer | Layer 7 (Application) | Layer 4 (Transport) |
| Protocols | HTTP, HTTPS, WebSocket | TCP, UDP, TLS |
| Routing Type | Content-based (URL, Host) | IP-based (no content awareness) |
| Intelligence | Smart routing | Fast routing |

---

## 🔹 Key Features

### ✅ ALB Features
- URL-based routing (`/api`, `/login`)
- Host-based routing (domain-based)
- Supports HTTP/HTTPS only
- Supports WebSockets
- Integrated with AWS WAF
- Good for microservices architecture
- Can route based on headers and query strings

---

### ✅ NLB Features
- Supports TCP, UDP, TLS
- Ultra-high performance and low latency
- Can handle millions of requests per second
- Provides static IP or Elastic IP
- Preserves source IP
- Ideal for real-time and high-throughput apps

---

## 🔹 Performance

| Feature | ALB | NLB |
|--------|-----|-----|
| Speed | Moderate | Very High |
| Latency | Higher | Very Low |
| Throughput | Medium | Extremely High |

---

## 🔹 Use Cases

### 🚀 Use ALB When:
- You are building web applications (HTTP/HTTPS)
- You need path-based routing (`/api`, `/admin`)
- You are using microservices
- You want integration with WAF
- You need smart routing decisions

---

### ⚡ Use NLB When:
- You need ultra-low latency
- You are handling TCP/UDP traffic
- You need static IP address
- You are building real-time systems (gaming, streaming)
- You need to handle millions of requests/sec

---

## 🔹 Static IP

| Feature | ALB | NLB |
|--------|-----|-----|
| Static IP | ❌ No | ✅ Yes (Elastic IP supported) |

---

## 🔹 Security

| Feature | ALB | NLB |
|--------|-----|-----|
| AWS WAF Support | ✅ Yes | ❌ No |
| SSL Termination | ✅ Yes | ✅ Yes (TLS) |

---

## 🔹 Health Checks

| Feature | ALB | NLB |
|--------|-----|-----|
| Health Check Type | HTTP/HTTPS | TCP/HTTP |
| Advanced Routing | ✅ Yes | ❌ No |

---

## 🔹 Target Types

| Feature | ALB | NLB |
|--------|-----|-----|
| EC2 | ✅ Yes | ✅ Yes |
| IP | ✅ Yes | ✅ Yes |
| Lambda | ✅ Yes | ❌ No |

---

## 🔹 Quick Summary

- **ALB = Smart + Flexible + Layer 7**
- **NLB = Fast + High Performance + Layer 4**

---

## 🔹 One-Line Decision

👉 Use **ALB** for web apps and smart routing  
👉 Use **NLB** for high performance and low latency

---

## 🔹 Easy Memory Trick

- **ALB → Application → Smart**
- **NLB → Network → Fast**

---
