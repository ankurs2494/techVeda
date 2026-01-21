---
title: "Phase 4 – DNS (Domain Name System)"
date: 2026-01-21
type: page
---

<!-- ### What This Phase Covers -->
DNS is one of the most critical systems on the internet.

If DNS fails, **everything appears broken** — even when the network is perfectly fine.

In this phase, we focus on DNS — the quiet system that makes the internet usable for humans.

You’ll learn how domain names are translated into IP addresses, what actually happens when you type a website into your browser, and how DNS queries move across multiple servers behind the scenes.

By the end of this phase, DNS should feel practical and intuitive, not mysterious. You’ll be able to understand DNS behavior in real situations and reason through common DNS-related problems with confidence.


---

### <font color="BlueViolet">1️⃣ Why DNS Exists</font>

#### The Problem

* Humans prefer names: `google.com`
* Computers require numbers: `142.250.72.14`

#### The Solution

**DNS translates names → IP addresses**

Without DNS:

* You would need to memorize IP addresses
* The internet would be effectively unusable

---

### <font color="BlueViolet">2️⃣ What Is DNS?</font>

**DNS (Domain Name System)** is a **distributed phone book** for the internet.

How it works:

* Name in → IP out
* Request → Response (just like all networking)

---

### <font color="BlueViolet">3️⃣ DNS Is NOT One Server</font>

DNS is a **hierarchical system**, not a single machine.

#### DNS Hierarchy Levels

* Root servers (`.`)
* TLD servers (`.com`, `.org`, `.net`)
* Authoritative servers (domain owners)

There is **no single point of failure** in DNS.

#### DNS Hierarchy (ASCII Diagram)

```
[ Root (.) ]
     |
     v
[ TLD (.com) ]
     |
     v
[ Authoritative (example.com) ]
```

---

### <font color="BlueViolet">4️⃣ DNS Resolution Flow</font> <font color="red">(Very Important)</font>

When you type:

```
www.example.com
```

#### Step-by-Step Resolution

1. Browser checks its local cache
2. Operating system checks DNS cache
3. Request goes to a **recursive resolver** (ISP / Cloud DNS)
4. Resolver performs iterative queries:

   * Root server → "Who handles .com?"
   * TLD server → "Who handles example.com?"
   * Authoritative server → "What is the IP?"
5. IP address is returned to the browser
6. Browser connects to the server using the IP

#### DNS Resolution Flow (ASCII Diagram)

```
Browser
   |
   v
Local Cache
   |
   v
Recursive Resolver
   |
   v
Root (.) → TLD (.com) → Authoritative (example.com)
   |
   v
IP Address Returned
```

💡 All of this happens in **milliseconds**.

---

### <font color="BlueViolet">5️⃣ DNS Records (You MUST Know These)</font>

| Record Type | Purpose               | Example                                                 |
| ----------- | --------------------- | ------------------------------------------------------- |
| A           | Name → IPv4           | example.com → 93.184.216.34                             |
| AAAA        | Name → IPv6           | example.com → IPv6 address                              |
| CNAME       | Alias to another name | [www.example.com](http://www.example.com) → example.com |
| MX          | Mail servers          | mail.example.com                                        |
| TXT         | Text data             | SPF, DKIM, verification                                 |

---

### <font color="BlueViolet">6️⃣ DNS Uses UDP (Mostly)</font>

#### Default behavior:

* Protocol: UDP
* Port: 53
* Fast and lightweight

#### DNS uses <font color="blue">**TCP**</font> when:

* Response size is large
* Zone transfers occur
* DNSSEC is enabled

---

### <font color="BlueViolet">7️⃣ TTL (Time To Live)</font>

**TTL** defines how long a DNS response can be cached.

#### Example

```
TTL = 300 seconds
```

#### Means:

* DNS changes are not seen immediately
* DNS problems may persist until cache expires

---

### <font color="BlueViolet">8️⃣ Internal vs Public DNS</font>

#### Public DNS

* Used for public websites
* Example: `google.com`
* Resolved via internet DNS infrastructure

#### Internal DNS

* Used inside companies or cloud environments
* Example: `service.internal`
* Not accessible from the public internet

---

### <font color="BlueViolet">9️⃣ Common DNS Problems (Real World)</font>

* Wrong DNS record
* Cached stale records (TTL issues)
* DNS server unreachable
* Misconfigured CNAME chains

> Most "network outages" are actually **DNS issues**.

---

### <font color="BlueViolet">🔟 DNS Is Layer 7</font> (But Affects Everything)

#### DNS itself is:

* Application Layer (Layer 7)

#### But when DNS fails, it looks like:

* Network issue
* Application issue
* Timeout or server down

This is what makes DNS **tricky to debug**.

---

### <font color="teal">Mental Model (Lock This In)</font>

```
Name → DNS → IP → TCP → Application
```

**No DNS = No Internet**

---

### <font color="teal">Check Your Understanding</font>

You should confidently answer:

* Why DNS exists
* Difference between A and CNAME records
* What TTL means
* Why DNS primarily uses UDP
* Why DNS issues are hard to debug
