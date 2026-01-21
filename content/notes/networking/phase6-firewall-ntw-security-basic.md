---
title: "Phase 6 : Firewall and Network Security Basics"
date: 2026-01-21
type: page
---

Phase 6 is where you learn why traffic is allowed or blocked. Most real outages live here.

This phase introduces the fundamentals of network security, focusing on how firewalls control and protect traffic. 

It explains basic security rules, filtering concepts, and how networks defend against unauthorized access and common threats.

---

### <font color="BlueViolet">1️⃣ What Is a Firewall?</font>

#### Simple definition:
    A firewall is a traffic filter.
    
#### It decides:
  - Who can talk
  - To whom
  - On which port
  - Using which protocol

Nothing more, nothing less.

---

### <font color="BlueViolet">2️⃣ Where Firewalls Exist</font> <font color="red">(Very Important)</font>

> **Tip:** Traffic must pass all firewalls in the path.


---

### <font color="BlueViolet">3️⃣ Firewall Rules (Core Concepts)</font>

Every firewall rule has:

| Field | Meaning  |
|-----------|---------|
| Direction | Inbound/Outbound |
| Source | Who is sending |
| Destination | Who is receiving |
| Port | Service  |
| Protocol | TCP/UDP |
| Action | Allow/Deny |

---

### <font color="BlueViolet">4️⃣ Inbound vs Outbound (Critical)</font>

<font color="teal">**Inbound rules**</font>
#### &nbsp;&nbsp;&nbsp;&nbsp; Control:   
&nbsp;&nbsp;&nbsp;&nbsp; Who can reach your service
    
#### &nbsp;&nbsp;&nbsp;&nbsp; Example:   
&nbsp;&nbsp;&nbsp;&nbsp; Allow TCP 443 from 0.0.0.0/0

<font color="teal">**Outbound rules**</font>

#### &nbsp;&nbsp;&nbsp;&nbsp; Control:   
&nbsp;&nbsp;&nbsp;&nbsp; Where your host can connect
    
#### &nbsp;&nbsp;&nbsp;&nbsp; Example:   
&nbsp;&nbsp;&nbsp;&nbsp; Allow TCP 443 to Internet

---

### <font color="BlueViolet">5️⃣ Default Firewall Behavior</font>

#### Most firewalls:

&nbsp;&nbsp;&nbsp;&nbsp; ❌ Deny inbound by default.  
&nbsp;&nbsp;&nbsp;&nbsp;✔ Allow outbound by default.  
    
> Zero-Trust flips this.

---

### <font color="BlueViolet">6️⃣ Firewalls Work at Layer 3 & 4</font>

#### Firewalls typically check:

&nbsp;&nbsp;&nbsp;&nbsp; - IP addresses (Layer 3).  
&nbsp;&nbsp;&nbsp;&nbsp; - Ports & protocols (Layer 4).  
    
#### They do NOT care:

&nbsp;&nbsp;&nbsp;&nbsp; - What app you run.  
&nbsp;&nbsp;&nbsp;&nbsp; - What data you send.  

---

### <font color="BlueViolet">7️⃣ Why Ping Works but Apps Don’t (Again)</font>

#### Because:
  - ICMP allowed
  - TCP/UDP blocked

    Ping = Reachability
    Ping ≠ service availability.

---

### <font color="BlueViolet">8️⃣ Stateful vs Stateless Firewalls</font>

<font color="teal">→ Stateless</font>
 
 &nbsp;&nbsp;&nbsp;&nbsp;- Each packet checked individually.  
 &nbsp;&nbsp;&nbsp;&nbsp;- No memory.  
    
<font color="teal">→ Stateful (Most modern firewalls)</font>

&nbsp;&nbsp;&nbsp;&nbsp;- Track connections.  
&nbsp;&nbsp;&nbsp;&nbsp;- Allow response traffic automatically.  

---

### <font color="BlueViolet">9️⃣ Common Firewall Scenarios</font>

#### <font color="teal"> Scenario 1: Web Server Not Reachable</font>

#### &nbsp;&nbsp;&nbsp;&nbsp; Symptoms:

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; - Service running.  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; - Port listening.  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; - Still unreachable.  

#### &nbsp;&nbsp;&nbsp;&nbsp; Cause:    
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; - Inbound rule missing

#### &nbsp;&nbsp;&nbsp;&nbsp; Fix:   
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; - Allow TCP 80/443

#### <font color="teal"> Scenario 2: No Internet Access</font>

#### &nbsp;&nbsp;&nbsp;&nbsp; Symptoms:   
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; - Cannot reach external sites

#### &nbsp;&nbsp;&nbsp;&nbsp; Cause:   
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; - Outbound blocked

#### &nbsp;&nbsp;&nbsp;&nbsp; Fix:   
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; - Allow outbound traffic

#### <font color="teal"> Scenario 3: Works Locally, Not Remotely</font>

#### &nbsp;&nbsp;&nbsp;&nbsp; Cause:

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; - Firewall allows localhost.  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; - Blocks external IPs.  

---

### <font color="BlueViolet">🔐 Network Security Basics</font>

**Principle of Least Privilege**

Allow only what is required
    
- Bad: Allow all ports from 0.0.0.0/0
- Good: Allow TCP 443 from trusted CIDR

**Defense in Depth**

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  Multiple layers:  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; - Host firewall.  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; - Cloud firewall.  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; - App-level auth. 

---

### <font color="BlueViolet">Mental Model (Lock This In)</font>
```
Packet
 ↓
Cloud Firewall ↓ Network Route ↓ Host Firewall ↓ Service
```

> Blocked anywhere = fail.

---

### <font color="BlueViolet">Interview Gold Statements</font>

- “Firewalls operate at Layer 3 and 4.”
- “Ping success doesn’t guarantee port access.”
- “Inbound is denied by default.”
- “Zero-Trust minimizes allowed paths.”

---

### <font color="BlueViolet">Check Your Understanding</font>

You should confidently answer:  
- Difference between inbound and outbound rules
- Why stateful firewalls matter
- Why security groups don’t need return rules
- Why localhost access bypasses firewall