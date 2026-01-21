---
title: "Phase 3 : TCP/IP and ports"
date: 2026-01-21
type: page
---

This phase is where networking becomes practical and real. We focus on how data actually moves between devices.

You’ll learn why TCP and UDP exist, how ports work, and how reliable and fast communication is achieved between clients and servers in the real world.

---

### <font color="BlueViolet">1️⃣ Why TCP/IP Exists</font>

**IP (Layer 3)** can only do one thing:

* Deliver packets from one IP to another IP

**Problems with IP alone:**

* No guarantee of delivery
* No ordering of packets
* No error recovery

Because of these limitations, we need **Transport Layer protocols**.

That’s where **TCP and UDP (Layer 4)** come in.

---

### <font color="BlueViolet">2️⃣ What Is a Port?</font> <font color="red">(Very Important)</font>

**Definition:**
A port identifies which application or service on a host should receive the data.

**Think of it like this:**

* IP = apartment building
* Port = apartment number

**Example:**

```
192.168.1.10:80
```

**Means:**

* Host: `192.168.1.10`
* Service: Web server (HTTP)

---

### <font color="BlueViolet">3️⃣ Why Ports Are Needed</font>

#### One single IP address can run multiple services at the same time:

* Web server
* SSH server
* Database
* Mail server

> <font color="blue">Ports allow multiple services to coexist on one IP address.</font>

---

### <font color="BlueViolet">4️⃣ Well-Known Ports (Memorize These)</font>

| Port | Service |
| ---- | ------- |
| 80   | HTTP    |
| 443  | HTTPS   |
| 22   | SSH     |
| 21   | FTP     |
| 25   | SMTP    |
| 53   | DNS     |

---

### <font color="BlueViolet">5️⃣ TCP (Transmission Control Protocol)</font>

#### What TCP Guarantees

* Reliable delivery
* Ordered packets
* Error detection
* Flow control

#### TCP Handshake (Conceptual)

```
Client → SYN
Server → SYN-ACK
Client → ACK
```

#### After this:

* Connection is established
* Data flows reliably

#### TCP Use Cases

* Web (HTTP/HTTPS)
* SSH
* Databases
* APIs

> <font color="red">When accuracy matters, use TCP.</font>

---

### <font color="BlueViolet">6️⃣ UDP (User Datagram Protocol)</font>

#### What UDP Does NOT Guarantee

* No delivery guarantee
* No packet order
* No retransmission

#### Why Use UDP?

* Faster
* Lower overhead
* Real-time communication

#### UDP Use Cases

* DNS
* Video streaming
* Online gaming
* VoIP

> <font color="red">When speed matters more than accuracy, use UDP.</font>

---

### <font color="BlueViolet">7️⃣ TCP vs UDP (Simple Table)</font>

| Feature     | TCP      | UDP       |
| ----------- | -------- | --------- |
| Reliability | Yes      | No        |
| Ordering    | Yes      | No        |
| Speed       | Slower   | Faster    |
| Connection  | Stateful | Stateless |
| Use Case    | Accuracy | Real-time |

---

### <font color="BlueViolet">8️⃣ Socket = IP + Port + Protocol</font>

A **socket** uniquely identifies a network connection:

```
IP + Port + Protocol
```

**Example:**

```
TCP 192.168.1.10:443
```

---

### <font color="BlueViolet">9️⃣ Client–Server Communication</font> (Real Flow)

#### When you open a website:

1. Browser chooses a random client port (e.g. `52134`)
2. Server listens on a well-known port (e.g. `443`)
3. TCP connection is established
4. Data is exchanged
5. Connection is closed

**Example:**

```
Client: 10.0.0.5:52134
Server: 142.250.72.14:443
```

---

### <font color="BlueViolet">🔟 Why Firewalls Use Ports</font>

#### Firewalls decide:

* Which IPs can talk
* On which ports
* Using which protocol

**Example rule:**

```
Allow TCP 443
Deny all others
```

---

### <font color="teal"> Mental Model (Lock This In)</font>

```
IP       → Finds the host
Port     → Finds the service
TCP/UDP  → Defines how data is sent
```

---

### <font color="teal"> Check Your Understanding</font>

#### You should be able to answer:

* Why are ports needed?
* What is the difference between TCP and UDP?
* What is a socket?
* Why does DNS use UDP?
* Why does HTTP use TCP?
