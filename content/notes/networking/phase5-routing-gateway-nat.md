---
title: "Phase 5 : Routing, Gateway and NAT"
date: 2026-01-21
type: page
---

Phase 5 focuses on how networks communicate. 

It covers routing, which directs traffic between networks; gateways, which connect different network types; and NAT (Network Address Translation), which lets multiple devices share a single IP while keeping internal addresses private.

---

### <font color="BlueViolet">1️⃣ What Is Routing?</font>

#### Definition:
Routing is the process of deciding where to send a packet next.

#### Every packet asks:
“How do I reach this destination IP?”

<font color="green">The answer comes from a routing table.</font>

--- 

### <font color="BlueViolet">2️⃣ Routing Table (Concept)</font>

#### A routing table is just a list of rules:

The most important rule:
    0.0.0.0/0 → default gateway

#### This means:
“If I don’t know where to send traffic, send it here.”

--- 

### <font color="BlueViolet">3️⃣ What Is a Gateway?</font>

#### Definition:
A gateway is a router that connects one network to another.

#### Your host says:
-  “This IP is not in my subnet”
-  “Send it to the gateway”

#### Without a gateway:
- ❌ You can only talk inside your subnet
- ❌ No internet access

--- 

### <font color="BlueViolet">4️⃣ Simple Example</font>

#### Host:
IP: 192.168.1.10/24  
Gateway: 192.168.1.1

-  To 192.168.1.20 → direct
-  To 8.8.8.8 → gateway

### <font color="BlueViolet">5️⃣ Routing Across the Internet</font>

--- 

#### When sending traffic to the internet:

a. Your host → local gateway
b. Gateway → ISP router
c. ISP → backbone routers
d. Reaches destination network
e. Response follows route back

#### Each router only knows:
“Where to send this packet next”

> No router knows the whole internet.

--- 

### <font color="BlueViolet">6️⃣ What Is NAT?</font>

#### Definition:
NAT (Network Address Translation) changes IP addresses in packets.

#### Why NAT exists:
- IPv4 address shortage
- Private IPs not internet-routable

--- 

### <font color="BlueViolet">7️⃣ Types of NAT (Conceptual)</font>

#### Source NAT (SNAT)
- Used for outbound internet access
- Private IP → Public IP

#### Destination NAT (DNAT)
- Used for inbound access
- Public IP → Private IP

--- 

### <font color="BlueViolet">8️⃣ NAT in Real Life</font>

#### Without NAT (Fails):
    192.168.1.10 → Internet ❌

#### With NAT (Works):
    192.168.1.10
> ↓ NAT Router (Public IP) ↓ Internet

#### NAT keeps a mapping table:
Private IP:Port ↔ Public IP:Port

--- 

### <font color="BlueViolet">9️⃣ Cloud Example</font> (Very Important)

#### Private subnet:
- Has private IPs only
- ❌ No internet by default

#### Add NAT Gateway:
- Outbound internet works
- Inbound still blocked

#### Public subnet:
- Has route to Internet Gateway
- Can receive inbound traffic

> Same networking rules, cloud names.

--- 

### <font color="BlueViolet">🔟 Routing vs NAT (Key Difference)</font>
| Routing                | NAT                  |
|------------------------|--------------------|
| Decides where packets go | Changes IP addresses |
| Exists everywhere       | Used at boundaries  |
| Layer 3                 | Layer 3/4           |


--- 

### <font color="teal"> Mental Model (Lock This In)</font>
    Host → Routing table → Gateway → NAT → Internet

--- 

### <font color="teal"> Common Beginner Mistakes</font>
- ❌ Thinking NAT = routing
- ❌ Forgetting default gateway
- ❌ Assuming private IPs are internet reachable
- ❌ Debugging firewall before routing

--- 

### <font color="teal"> Check Your Understanding</font>
You should confidently answer:
- What is routing?
- What is a default gateway?
- Why NAT is required?
- Difference between SNAT and DNAT?
- Why private subnets need NAT gateways?
