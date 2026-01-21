---

title: "Content Delivery Network (CDN): Beginner to SRE-Level Complete Guide"
date: 2026-01-04
author: "Ankur Singh"
description: "A complete CDN guide from basics to advanced SRE concepts with diagrams, use cases, examples, and 20 interview Q&A."
tags: ["cdn", "caching", "sre", "system-design", "interview", "web-performance"]
--------------------------------------------------------------------------------

## Introduction

A **Content Delivery Network (CDN)** is a core building block of modern, scalable, and reliable web systems. From simple websites to global-scale platforms like Netflix and Cloudflare, CDNs are essential for **performance, availability, and security**.

This guide starts from **absolute basics** and progresses to **advanced SRE-level concepts**, including cache strategies, failure handling, and real-world operations.

---

## What is a CDN? (Beginner Level)

A CDN is a **globally distributed network of servers** that deliver content to users from the **nearest geographical location**.

Instead of every user hitting your origin server, content is served from **edge locations**, reducing latency and load.

### Simple Example

* User in India requests `image.png`
* Image is served from Mumbai edge instead of US origin

---

## Basic CDN Flow Diagram

```
User
 │ Request Content
 ▼
Nearest CDN Edge
 │ Cache Hit?
 ├── Yes → Serve Content
 └── No
        ▼
     Origin Server
        │ Fetch Content
        ▼
     CDN Edge (Cache)
        │
        ▼
       User
```

---

## What Does a CDN Cache?

* Static assets (JS, CSS, images, videos)
* HTML pages
* API responses (with rules)
* Media streams

---

## Key Benefits of CDN

### Performance

* Reduced latency
* Faster page load times

### Scalability

* Handles traffic spikes
* Offloads origin servers

### Reliability

* Origin protection
* Failover and redundancy

### Security

* DDoS protection
* WAF integration
* TLS termination

---

## CDN Use Cases

### Beginner / Common

* Website static asset delivery
* Image & video hosting

### Intermediate

* API acceleration
* Mobile app backend caching

### Advanced / SRE-Level

* Global SaaS platforms
* Live streaming
* Zero-downtime deployments
* Edge computing

---

## Real-Life CDN Examples

* **Netflix**: Video streaming from edge locations
* **Amazon CloudFront**: E-commerce asset delivery
* **Cloudflare**: CDN + security platform
* **YouTube**: Adaptive video delivery

---

## CDN Caching Strategies (Intermediate)

* Cache-Control headers
* TTL-based caching
* Cache busting (versioned URLs)
* Purge / Invalidation

### Example Header

```http
Cache-Control: public, max-age=3600
```

---

## Advanced CDN Concepts (SRE Level)

### Edge Computing

* Run logic at CDN edge (Cloudflare Workers, Fastly Compute)

### Origin Shield

* Single mid-tier cache to protect origin

### Cache Hierarchy

* Edge → Regional → Origin

### Failover & Multi-CDN

* Traffic switching on failure
* Vendor lock-in avoidance

---

## Advanced CDN Architecture Diagram

```
Users (Global)
      │
      ▼
 CDN Edge Locations
      │
      ▼
 Regional Cache / Origin Shield
      │
      ▼
 Origin Infrastructure
 (Load Balancer + Services + DB)
```

---

## CDN vs Traditional Cache (Redis/Memcached)

| Aspect    | CDN           | Redis/Memcached   |
| --------- | ------------- | ----------------- |
| Location  | Edge (Global) | Application Layer |
| Data Type | HTTP Content  | Application Data  |
| Latency   | Internet Edge | In-Process / VPC  |
| Security  | Built-in      | App-managed       |

---

## Common CDN Pitfalls

* Incorrect cache headers
* Serving stale content
* Cache poisoning
* Over-aggressive caching

---

## Interview Questions & Answers (20 Must-Know)

<details>
<summary><strong>1. What is a CDN?</strong></summary>
A CDN is a distributed network that serves content from locations closest to users.
</details>

<details>
<summary><strong>2. Why do we use CDNs?</strong></summary>
To reduce latency, improve availability, and protect origin servers.
</details>

<details>
<summary><strong>3. What is an edge location?</strong></summary>
A CDN server that delivers content close to users.
</details>

<details>
<summary><strong>4. What is an origin server?</strong></summary>
The main backend server where original content resides.
</details>

<details>
<summary><strong>5. What is cache hit and miss?</strong></summary>
Hit means content served from cache; miss means fetched from origin.
</details>

<details>
<summary><strong>6. What headers control CDN caching?</strong></summary>
Cache-Control, Expires, ETag, Last-Modified.
</details>

<details>
<summary><strong>7. What is cache invalidation?</strong></summary>
Removing cached content before TTL expires.
</details>

<details>
<summary><strong>8. CDN vs Load Balancer?</strong></summary>
CDN serves cached content; load balancer distributes traffic to backends.
</details>

<details>
<summary><strong>9. Can CDNs cache dynamic content?</strong></summary>
Yes, with rules, headers, and edge logic.
</details>

<details>
<summary><strong>10. What is edge computing?</strong></summary>
Executing code closer to users at CDN edge locations.
</details>

<details>
<summary><strong>11. What is Origin Shield?</strong></summary>
A centralized cache layer protecting origin servers.
</details>

<details>
<summary><strong>12. How do CDNs help with DDoS?</strong></summary>
By absorbing and filtering traffic at the edge.
</details>

<details>
<summary><strong>13. What is multi-CDN?</strong></summary>
Using multiple CDN providers for resilience and performance.
</details>

<details>
<summary><strong>14. What is cache poisoning?</strong></summary>
Injecting malicious content into CDN cache.
</details>

<details>
<summary><strong>15. What is stale-while-revalidate?</strong></summary>
Serving stale content while fetching fresh data in background.
</details>

<details>
<summary><strong>16. How does CDN failover work?</strong></summary>
Traffic is rerouted to healthy edges or another CDN.
</details>

<details>
<summary><strong>17. What metrics do SREs monitor for CDN?</strong></summary>
Cache hit ratio, latency, error rate, origin load.
</details>

<details>
<summary><strong>18. CDN vs Reverse Proxy?</strong></summary>
CDN is globally distributed; reverse proxy is usually local.
</details>

<details>
<summary><strong>19. What happens if CDN is down?</strong></summary>
Traffic can fall back to origin or another CDN.
</details>

<details>
<summary><strong>20. One-line CDN summary?</strong></summary>
CDNs bring content closer to users for speed, scale, and security.
</details>

---

## Conclusion

For SREs, CDNs are not just about speed—they are critical infrastructure for **reliability, scalability, and security**. Mastering CDN internals is essential for operating global systems.

