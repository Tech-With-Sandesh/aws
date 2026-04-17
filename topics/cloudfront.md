# 🚀 AWS CloudFront Debugging Guide: Users Still See Old Content

## 📌 Overview

A very common production issue:

> AWS CloudFront is enabled, but users still see old (stale) content after deployment.

This happens due to **caching behavior**, not deployment failure.

---

## 🧠 How CloudFront Works (Quick)

CloudFront caches content at edge locations.

Flow:
```
User → CloudFront Edge → (Cache Hit) → Response  
                        → (Cache Miss) → Origin (S3 / ALB / EC2)
```

---

## 🎯 Goal

Identify:
- Why stale content is served
- How to refresh/update cache
- Prevent future issues

---

## 🔍 Step 1: Check Cache Behavior (Most Common Issue)

CloudFront caches content based on TTL (Time To Live).

### Check:
- Default TTL
- Maximum TTL
- Minimum TTL

### Problem:
- TTL is too high → old content served

### Fix:
- Reduce TTL in behavior settings
- Or control via headers (recommended)

---

## ⚠️ Step 2: Cache Invalidation Not Done

### Problem:
New deployment done, but cache not invalidated

### Fix:

```bash
aws cloudfront create-invalidation \
  --distribution-id <DIST_ID> \
  --paths "/*"
```

👉 This forces CloudFront to fetch fresh content

---

## 🔍 Step 3: Check Cache-Control Headers

CloudFront respects origin headers.

### Example:

```http
Cache-Control: max-age=86400
```

### Problem:
Browser + CloudFront caches for long time

### Fix:

```http
Cache-Control: no-cache, no-store, must-revalidate
```

Or for controlled caching:

```http
Cache-Control: max-age=60
```

---

## 🔍 Step 4: Browser Cache (Often Ignored)

Even after invalidation, browser may cache content.

### Fix:
- Hard refresh (Ctrl + Shift + R)
- Test in incognito mode

---

## 🔍 Step 5: Origin Not Updated

### Problem:
CloudFront is correct, but origin still serves old content

### Check:
- Directly hit origin (S3 / EC2 / ALB)

```bash
curl <origin-url>
```

### Fix:
- Ensure deployment updated origin

---

## 🔍 Step 6: Check S3 Versioning / Object Names

### Problem:
Same file name → cache confusion

Example:
```
index.html (same name, new content)
```

### Fix:
Use versioned files:
```
app-v2.js
style-v3.css
```

---

## 🔍 Step 7: Query String / Headers Not Forwarded

### Problem:
CloudFront ignores query params → same cached response

### Fix:
- Enable query string forwarding
- Configure cache key properly

---

## 🔍 Step 8: Multiple Cache Layers

### Problem:
Caching exists at:
- CloudFront
- Browser
- CDN
- API Gateway

### Fix:
- Identify all cache layers
- Clear/invalidate accordingly

---

## 🔍 Step 9: Check CloudFront Cache Status

Use headers:

```bash
curl -I https://your-domain.com
```

Look for:

```
X-Cache: Hit from cloudfront
```

or

```
X-Cache: Miss from cloudfront
```

---

## 📊 Common Root Causes

| Issue | Impact |
|------|--------|
| High TTL | Old content served |
| No invalidation | Cache not refreshed |
| Cache-Control headers | Long caching |
| Browser cache | Local stale content |
| Origin not updated | Old data source |
| Same file names | Cache confusion |

---

## 🎯 Final Debugging Checklist

- [ ] Cache invalidation done
- [ ] TTL configured properly
- [ ] Cache-Control headers verified
- [ ] Browser cache cleared
- [ ] Origin updated correctly
- [ ] Versioned files used
- [ ] Query string forwarding configured

---

## 🚀 Best Practices

- Use versioned file names (cache busting)
- Set proper Cache-Control headers
- Automate cache invalidation in CI/CD
- Keep low TTL for dynamic content
- Use high TTL for static assets

---

## 📌 Conclusion

CloudFront serving old content is NOT a deployment issue.

It is caused by:
- Caching behavior
- TTL configuration
- Missing invalidation

Once you understand caching layers, this becomes easy to debug 🚀

---

## ⭐ If this helped

- Star ⭐ the repo  
- Share with others  
- Follow for more DevOps content  

---
