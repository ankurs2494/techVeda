---
title: "Phase 2 : IP and Subnetting"
date: 2026-01-20
type: page
---

In this phase, we explore IP addresses and subnetting — the part of networking that explains how devices are identified and grouped together.

You’ll understand what IP addresses represent, why networks are divided into subnets, and how this structure makes large networks possible and manageable.

---

### <font color="orange">IP Addresses (The Heart of Networking)</font>


### <font color="blue">What Is an IP Address</font>
An IP address is a logical identifier that tells the network:
- Who you are
- Where you are

**Example:** 192.168.1.10

**Mental model:** Country → City → Street → House number

---

### <font color="blue">IPv4 Basics</font>

**IPv4 format:** A.B.C.D

**Each number:**
  - Range: 0–255
  - Called an octet  

**Structure:**
  - 4 octets
  - 32 total bits

**Example:** 192.168.1.10

---

### <font color="blue">Public vs Private IP Addresses</font>

### 1. Private IPs (Internal Use)

#### Reserved ranges:
  -  10.0.0.0/8
  -  172.16.0.0/12
  -  192.168.0.0/16

#### Used in:
- Home networks
- Office networks
- Cloud VPCs

#### Properties:
- Reused everywhere
- Exist simultaneously in millions of networks
- Not globally routable

> Private IPs cannot be reached directly from the internet.

---

### 2. Public IPs (Internet-Facing)

Properties:
- Globally unique
- Assigned by ISP or cloud provider
- Routable across the internet

---

### <font color="blue">Loopback Address</font>
    127.0.0.1


**Meaning:** This host talking to itself

**Used for:**
- Testing
- Local services

---

### <font color="blue">Learn Subnetting (Don’t Fear It)</font>

### What Is a Subnet

#### Definition:
- A subnet is a logical grouping of IP addresses.

#### Purpose:
- Organization
- Security
- Efficient routing

#### Example:
    192.168.1.0/24


---

### <font color="blue">Understanding CIDR (/24, /16, etc.)</font>

#### CIDR defines:
- How much of the IP is network
- How much is host

#### Example:
    192.168.1.0/24

#### Explanation:
- /24 = first 24 bits are network
- Remaining bits are host addresses

> CIDR sizes listed here are sufficient for beginners.

---

### <font color="blue">Network IP vs Host IP</font>

**For:** 192.168.1.0/24

- **Network address:** 192.168.1.0
- **Broadcast address:** 192.168.1.255
- **Usable hosts:** 192.168.1.1 – 192.168.1.254

---

### <font color="blue">Default Gateway (Critical Concept)</font>

#### What It Is
The router a host uses to reach other networks.

#### Routing logic:
- Same subnet → direct communication
- Different subnet → send to gateway

#### Example:
    IP: 192.168.1.10
    Gateway: 192.168.1.1


### Mental Model
    Host → Gateway → Internet → Gateway → Host


> No gateway means no external connectivity.

---

### <font color="blue">Why the Internet Works</font>

#### Requirements:
- Every device has a unique address
- Routers know where to forward traffic

#### Why routers drop private IP traffic:
- Private ranges are not globally unique
- They are reserved for internal use
- Internet routers do not route them

---

### <font color="blue">How Two Hosts Communicate</font>

**Same Subnet**: Direct communication

    192.168.1.10 → 192.168.1.20

**Different Subnet:** Traffic goes through the gateway

    192.168.1.10 → 8.8.8.8

---

### <font color="blue">Cloud Networking Example (AWS / Azure / GCP)</font>

- Private subnet only → No internet access
- Private subnet + NAT Gateway → Internet access
- Public subnet + Internet Gateway → Internet access

> Same rules apply everywhere.

---

### <font color="teal">Key Rules to Retain</font>

1. IP address = identity + location
2. Subnet = grouped IP space
3. Gateway = exit point to other networks
4. Private IPs are not internet-reachable
5. Loopback means self-communication

---

### <font color="teal">Knowledge Check</font>

### You must be able to answer:

- What is an IP address?
- Difference between public and private IPs?
- What does /24 represent?
- What is a default gateway?
- Why can’t private IPs access the internet directly?

### Correct reasoning:
- Private IPs are not globally unique
- They are not routable on the public internet
- NAT is required to translate them to public IPs