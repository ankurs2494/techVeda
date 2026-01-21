---

title: "Redis vs Memcached: Short Explanation, Use Cases, Pros & Cons"
date: 2026-01-04
author: "Ankur Singh"
description: "A concise comparison of Redis and Memcached with real-life scenarios and 20 must-know interview questions."
tags: ["redis", "memcached", "caching", "system-design", "interview"]
---------------------------------------------------------------------

## Introduction

Caching is a critical technique used to improve application performance and scalability. Two of the most popular in-memory caching systems are **Redis** and **Memcached**. While both are fast, they are designed for different use cases.

This post gives a **short, clear explanation**, followed by **real-world scenarios**, and ends with **20 interview questions and answers** worth knowing.

---

## Cache Flow Diagram

### Basic Cache-Aside Pattern (Used with Redis & Memcached)

```
Client
  │
  │ Request Data
  ▼
Application Server
  │
  │ 1. Check Cache
  ▼
Cache (Redis / Memcached)
  │
  │ Cache Miss
  ▼
Database
  │
  │ Fetch Data
  ▼
Cache (Store Result with TTL)
  │
  ▼
Application Server
  │
  ▼
Client (Response)
```

**Explanation:**

* Application first checks cache
* On miss, data is fetched from DB
* Result is stored in cache for future requests

---

## What is Redis?

Redis is an **in-memory data store** that can be used as a **cache, database, or message broker**. It supports multiple data structures and optional persistence.

### Common Use Cases

* Session storage
* API response caching
* Rate limiting
* Real-time analytics
* Leaderboards
* Pub/Sub messaging

### Example

```bash
SET user:123 "{name: 'Alice'}"
EXPIRE user:123 3600
```

### Pros

* Supports advanced data structures
* Optional persistence (RDB, AOF)
* Replication and high availability
* Atomic operations
* Pub/Sub support

### Cons

* Higher memory usage
* Slightly complex to manage
* Overkill for very simple caching

### Real-Life Scenarios

* **E-commerce**: inventory counters
* **Social media**: likes, followers, timelines
* **Gaming**: real-time leaderboards
* **Authentication systems**: sessions and tokens

---

## What is Memcached?

Memcached is a **lightweight, in-memory key-value store** designed purely for caching.

### Common Use Cases

* Page caching
* Database query caching
* Object caching

### Example

```bash
set user_123 3600 0 15
Alice
```

### Pros

* Extremely fast
* Simple to use
* Low memory overhead
* Easy horizontal scaling

### Cons

* No persistence
* No replication
* Only string key-value storage
* Data lost on restart

### Real-Life Scenarios

* **News websites**: homepage caching
* **Blogs**: rendered HTML caching
* **Read-heavy applications**: database load reduction

---

## Redis vs Memcached Comparison

| Feature                    | Redis    | Memcached    |
| -------------------------- | -------- | ------------ |
| Data Types                 | Multiple | Strings only |
| Persistence                | Yes      | No           |
| Replication                | Yes      | No           |
| Pub/Sub                    | Yes      | No           |
| Complexity                 | Medium   | Simple       |
| Performance (simple cache) | Fast     | Very fast    |

---

## When to Choose Which?

* Choose **Redis** if you need durability, complex data types, or real-time features.
* Choose **Memcached** if you want extremely fast, temporary caching with minimal setup.

---

## Interview Questions & Answers (20 Must-Know)

<details>
<summary><strong>1. What is caching?</strong></summary>
Caching is storing frequently accessed data in memory to reduce latency and backend load.
</details>

<details>
<summary><strong>2. Difference between Redis and Memcached?</strong></summary>
Redis supports persistence and data structures; Memcached is a simple key-value cache.
</details>

<details>
<summary><strong>3. Is Redis only a cache?</strong></summary>
No. Redis can act as a cache, database, and message broker.
</details>

<details>
<summary><strong>4. Why is Redis slower than Memcached in some cases?</strong></summary>
Redis has more features and metadata overhead.
</details>

<details>
<summary><strong>5. Does Redis support persistence?</strong></summary>
Yes, via RDB snapshots and AOF logs.
</details>

<details>
<summary><strong>6. What happens to Memcached data after restart?</strong></summary>
All data is lost.
</details>

<details>
<summary><strong>7. What data structures does Redis support?</strong></summary>
Strings, hashes, lists, sets, sorted sets, streams, and more.
</details>

<details>
<summary><strong>8. What is Redis Pub/Sub?</strong></summary>
A messaging pattern where publishers send messages to subscribers.
</details>

<details>
<summary><strong>9. Can Memcached replicate data?</strong></summary>
No, replication is not supported.
</details>

<details>
<summary><strong>10. What is cache eviction?</strong></summary>
Removing old data when cache memory is full.
</details>

<details>
<summary><strong>11. What eviction policies does Redis support?</strong></summary>
LRU, LFU, TTL-based, and no-eviction.
</details>

<details>
<summary><strong>12. Which is better for session storage?</strong></summary>
Redis, because it supports persistence and TTLs.
</details>

<details>
<summary><strong>13. Can Redis be used for rate limiting?</strong></summary>
Yes, using atomic counters and expiration.
</details>

<details>
<summary><strong>14. Is Memcached thread-safe?</strong></summary>
Yes, it supports multithreaded operations.
</details>

<details>
<summary><strong>15. What is TTL?</strong></summary>
Time-To-Live defines how long cached data stays valid.
</details>

<details>
<summary><strong>16. Can Redis scale horizontally?</strong></summary>
Yes, using Redis Cluster.
</details>

<details>
<summary><strong>17. Why use Memcached over Redis?</strong></summary>
For very simple, high-speed, ephemeral caching.
</details>

<details>
<summary><strong>18. Is Redis single-threaded?</strong></summary>
Mostly yes, but it uses I/O multiplexing and background threads.
</details>

<details>
<summary><strong>19. What happens if cache fails?</strong></summary>
The system falls back to the database (cache miss).
</details>

<details>
<summary><strong>20. Redis vs Memcached — one-line summary?</strong></summary>
Redis is feature-rich; Memcached is fast and minimal.
</details>

---

## Conclusion

Redis and Memcached both solve caching problems but target different needs. Understanding their strengths helps you design scalable, high-performance systems — and ace system design interviews.

