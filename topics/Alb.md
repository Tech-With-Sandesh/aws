# 🚀 AWS Debugging Guide: ALB Shows Healthy Targets but Users Get 502/504 Errors

## 📌 Overview

A very common production issue:

> Application is behind an AWS Application Load Balancer (ALB).  
> Health checks pass, targets are healthy, but users still receive **502 / 504 errors**.

This indicates a **mismatch between health checks and real request handling**.

---

## 🧠 What These Errors Mean

### 502 Bad Gateway
- ALB cannot properly communicate with backend
- Target returned invalid response

### 504 Gateway Timeout
- Target did not respond within timeout

---

## 🎯 Goal

Identify:
- Why ALB thinks targets are healthy
- Why real requests are failing
- Where the failure occurs (ALB ↔ App ↔ Dependency)

---

## 🔍 Step 1: Understand Health Check vs Real Traffic

Health checks are usually:
- Simple (e.g., `/health`)
- Lightweight
- Fast

Real traffic:
- Complex requests
- Database calls
- External APIs

👉 So a target can pass health checks but fail real requests.

---

## 🔍 Step 2: Check Target Response Time

### Problem:
Application is slow → exceeds ALB timeout

### Default:
- ALB idle timeout = 60 seconds

### Debug:
- Check application latency
- Review logs

### Fix:
- Optimize app performance
- Increase timeout (if needed)

---

## 🔍 Step 3: Check Backend Application Errors

### Problem:
App returns invalid response or crashes

### Debug:
- Check application logs
- Look for:
  - Exceptions
  - 5xx errors
  - Connection failures

### Tools:
- AWS CloudWatch Logs
- Application logs

---

## 🔍 Step 4: Check Target Port & Protocol

### Problem:
Mismatch between ALB and target

Example:
- ALB sends HTTP
- App expects HTTPS

### Fix:
- Match protocol and port correctly

---

## 🔍 Step 5: Check Security Groups

### Problem:
ALB cannot communicate with instances

### Fix:
- ALB SG → allow outbound
- EC2 SG → allow inbound from ALB SG

---

## 🔍 Step 6: Check Network ACLs

### Problem:
Traffic blocked at subnet level

### Fix:
- Allow inbound/outbound traffic
- Ensure ephemeral ports allowed

---

## 🔍 Step 7: Check Application Dependencies

### Problem:
App depends on:
- Database (RDS)
- External APIs

If dependency is slow → request fails

### Fix:
- Check DB latency
- Monitor API calls

---

## 🔍 Step 8: Check Keep-Alive / Connection Issues

### Problem:
- Connection closed prematurely
- Improper HTTP handling

---

## 🔍 Step 9: Check Load Balancer Timeout

### Problem:
Request takes longer than ALB timeout

### Fix:
- Increase idle timeout
- Optimize backend processing

---

## 🔍 Step 10: Check Scaling & Load

### Problem:
- Traffic spike
- Not enough instances

### Fix:
- Enable Auto Scaling
- Add more instances

---

## 📊 Common Root Causes

| Issue | Result |
|------|--------|
| Slow backend | 504 Timeout |
| App crash | 502 Error |
| Protocol mismatch | 502 Error |
| Dependency slowness | Timeout |
| Security group issue | Connection failure |
| Insufficient scaling | Errors under load |

---

## 🎯 Final Debugging Checklist

- [ ] Health check vs real endpoint validated
- [ ] Application logs checked
- [ ] Backend latency analyzed
- [ ] Security groups verified
- [ ] Timeout configuration reviewed
- [ ] Dependencies checked
- [ ] Scaling validated

---

## 🚀 Best Practices

- Use realistic health checks (not just `/health`)
- Monitor latency and error rates
- Configure proper timeouts
- Implement retries and circuit breakers
- Use autoscaling

---

## 📌 Conclusion

If ALB shows healthy targets but users get errors:

👉 The issue is usually in:
- Application logic  
- Backend performance  
- Dependencies  

NOT the load balancer itself.

---

## ⭐ Support

- Star ⭐ the repo  
- Share with others  
- Follow for more DevOps content  

---
