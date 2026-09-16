# Module 3 — Networking

## 1. What is a Network?

A **Network** is a collection of two or more devices connected together so they can **communicate and exchange data**.

Devices can include:

* 💻 Computers / Laptops
* 🖥️ Servers
* 📱 Mobile devices
* 🖨️ Printers
* 🔀 Switches
* 🌐 Routers
* 🔥 Firewalls
* ☁️ Cloud resources

### Simple Example

When you open:

`https://www.google.com`

your laptop communicates with Google's servers through multiple networks and networking devices.

---

## 2. Basic Network Architecture

![Image](https://images.openai.com/static-rsc-4/dAsp74sBiJ7GwThZrxDykp9ZscTimUgq4b-ml28avgNca9EARddotZy5iLrmmA1GvJAh1VUjXQgEJS-8bxUldthhwiebmkuhBJRJysw2oRZgnjlbKSh8IeowFkUq63QQ89xs6C-SkZwlqDqvqZRxIl76jnahdBp1Fb8XsWRbSlYyKmBwUaSX466mQpiZWAs_?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/GP-RnGp2bCcbjxqUlzLQl207SCE1svVruK1hVzCcp-tP6Ah63-_ljRKylqWmVIqYsU2junYFr2P-IBvFZks4Qc2I38WOe__sALIi-lIHmKLuoPvcZEwAvzYZsrRWOhgTAdeeAiDaOEfDJ6fVZ5y9PAMc8ApmHH-1Qv7ZboTYv9dsk0sjthQf5upncu8oQahr?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/EvDmvv8yQY2kjy5jit0R9OLddfxcBC49n0KKq0H0s-9rrMl0TRbJ0tOFy8I0hTL8i6aB7XMUnnS1Y7XabM6GhqBLorkfJ3a7Po9TkNOSjyY8DwV-H1SJ5MHKJavSOU6x2owyJ1u_I6Uzgx7XX_ROZkargT5mVgvKWRKH4lUtsQ5SJGHSGywQG2bKzVD-ZgX5?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/Mp0Fm2aKvs_PrCeJjMseTJvCkJ7xC92bTfXIp8KB7d2slW_aJ62_9xZpAbWXE25T6FHHNpiDhJmHXheCcZpKa1Qo86MToWxHTTs1WtFlyxVf4Y09tZqb8ZN3sDk4Ih__8ZoEnQ1aeiNYLW_yl3Oj_AxZE91Xnqdsaqqtcy_ZbL4JINo0XOjostxCccBPQGi3?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/GnylQZh5EAinRwtaFgWZ3vQYiCuKyU4GgbPLk25DKxt9yh7AodSLiKJmxLfV1bZkrFXWQER4_PuWSsDrz8C53tLQhkPK7i3IPaQBo7g6Oeitn4OP20KuZrGdBalZp-biQSBLoQh9GTuC2ci6u6l8Ji7f7UFyimgWmUvBqeR0Utbh0MRSMsaaxYGPfkwGF62_?purpose=fullsize)

A simplified architecture looks like this:

```text
                 YOUR NETWORK
        ┌──────────────────────────┐
        │                          │
     💻 Laptop                  📱 Mobile
        │                          │
        └──────────┬───────────────┘
                   │
                🔀 Switch
                   │
                🌐 Router
                   │
              🔥 Firewall
                   │
             ☁️ Internet
                   │
        ┌──────────┴───────────┐
        │                      │
   🌐 Web Server          🗄️ Database
```

### How communication happens

```text
Client
  ↓
Switch
  ↓
Router
  ↓
Firewall
  ↓
Internet
  ↓
Web Server
  ↓
Database
```

For example:

**User → Browser → Router → Internet → Web Server → Database → Response → User**

---

# 3. What is Networking?

**Networking** is the process of **designing, configuring, connecting, securing, and managing networks** so that devices and applications can communicate.

### Network vs Networking

| Network                      | Networking                                 |
| ---------------------------- | ------------------------------------------ |
| The connected infrastructure | Process of managing the infrastructure     |
| Devices connected together   | Connecting and configuring devices         |
| Example: LAN                 | Example: Configuring LAN                   |
| Physical/logical structure   | Technologies, protocols and administration |

### Easy way to remember

> **Network = What is connected?**
> **Networking = How do they communicate?**

---

# 4. Why Do We Need Networking?

Networking allows us to:

### 1. Communication

```text
Computer A ─────────→ Computer B
```

Exchange data between systems.

### 2. Internet Access

```text
Laptop → Router → ISP → Internet
```

### 3. Resource Sharing

Multiple users can access:

* Files
* Printers
* Applications
* Databases
* Servers

### 4. Application Communication

Modern applications usually have multiple components.

```text
User
 ↓
Frontend
 ↓
Backend API
 ↓
Database
```

All these components need networking.

### 5. Security

Networking allows us to control:

* Who can communicate
* Which ports are allowed
* Which systems can access databases
* Internet access
* Internal vs external traffic

---

# 5. Real-World Example — E-Commerce Application

Consider a website such as:

**QuickKart.com**

A user accesses the application:

```text
                  INTERNET
                     │
                     ↓
                🌐 Load Balancer
                     │
          ┌──────────┴──────────┐
          ↓                     ↓
     Web Server 1          Web Server 2
          │                     │
          └──────────┬──────────┘
                     ↓
                Backend API
                     │
                     ↓
                🗄️ Database
```

When the user clicks **"Buy Now"**:

```text
User
 ↓
Internet
 ↓
Load Balancer
 ↓
Frontend
 ↓
Backend API
 ↓
Database
 ↓
Response
 ↓
User
```

Networking makes this entire communication possible.

---

# 6. Important Networking Components

Students should understand these components first:

```text
                    NETWORK
                       │
       ┌───────────────┼────────────────┐
       │               │                │
    Switch           Router          Firewall
       │               │                │
       ↓               ↓                ↓
   LAN Network      Other Networks    Security
                       │
                       ↓
                    Internet
```

### 🔀 Switch

Connects devices within the **same local network**.

```text
PC1 ─┐
PC2 ─┼── Switch
PC3 ─┘
```

### 🌐 Router

Connects **different networks**.

```text
LAN 1
  │
Router
  │
LAN 2
```

It determines where network traffic should go.

### 🔥 Firewall

Controls network traffic based on security rules.

```text
Internet
    │
    ↓
 Firewall
    │
    ↓
Private Network
```

Example:

```text
Allow  → HTTPS : 443
Allow  → SSH   : 22
Deny   → Unknown Traffic
```

---

# 7. LAN, WAN and Internet

### LAN — Local Area Network

Network within a limited area.

Examples:

* Home
* Office
* School
* Data center

```text
PC ──┐
PC ──┼── Switch ── Router
PC ──┘
```

---

### WAN — Wide Area Network

Connects networks across large geographical areas.

```text
Office A
   │
Router
   │
   └──── WAN ────┐
                 │
              Router
                 │
              Office B
```

---

### Internet

A global network connecting networks around the world.

```text
Home Network
      │
      ↓
    ISP
      │
      ↓
  INTERNET
      │
 ┌────┴─────┐
 ↓          ↓
Google    Amazon
```

---

# 8. IP Address — Identity of a Device

An **IP address** identifies a device/interface on an IP network.

Example:

```text
192.168.1.10
```

Think of it like a **postal address for network communication**.

```text
Device A                  Device B
192.168.1.10              192.168.1.20
     │                         │
     └──────── Network ────────┘
```

For cloud environments, IP addressing becomes extremely important.

---

# 9. Public IP vs Private IP

### Private IP

Used inside private networks.

Examples:

```text
10.0.0.10
172.16.1.10
192.168.1.10
```

Typical architecture:

```text
Internet
   │
Public IP
   │
Router / Firewall
   │
Private IP
   │
Server
```

### Public IP

An IP address reachable from the public Internet, subject to routing and firewall/security controls.

Example:

```text
Client
  │
Internet
  │
Public IP
  │
Web Server
```

---

# 10. Port — Application Door

An **IP address identifies the destination**, while a **port identifies the network service/application endpoint** on that destination.

Common ports:

| Port | Protocol / Service      |
| ---: | ----------------------- |
|   22 | SSH                     |
|   53 | DNS                     |
|   80 | HTTP                    |
|  443 | HTTPS                   |
| 3306 | MySQL                   |
| 5432 | PostgreSQL              |
| 8080 | Common application port |

Example:

```text
Server IP
10.0.1.10

      ↓

10.0.1.10:443
        │
        └── HTTPS
```

So:

> **IP = Which machine/network interface?**
> **Port = Which service?**

---

# 11. Protocol — Rules of Communication

A **network protocol** defines rules for how devices communicate.

Common protocols:

```text
HTTP / HTTPS → Web communication
DNS          → Name resolution
SSH          → Secure remote access
TCP          → Reliable transport
UDP          → Connectionless transport
ICMP         → Network diagnostics
DHCP         → IP configuration
```

Example:

```text
Browser
   │
 HTTPS
   │
   ↓
Web Server
```

---

# 12. DNS — Name to IP Resolution

Humans prefer names:

```text
www.example.com
```

Networks use IP addresses.

DNS translates the name into an IP address.

```text
User
 │
 │ www.example.com
 ↓
DNS Server
 │
 │ 93.x.x.x
 ↓
Web Server
```

Simple analogy:

> **DNS is like the Internet's phonebook.**

---

# 13. TCP/IP — Foundation of Modern Networking

A simplified TCP/IP model:

```text
┌─────────────────────────────┐
│ Application                 │
│ HTTP, HTTPS, DNS, SSH       │
├─────────────────────────────┤
│ Transport                   │
│ TCP, UDP                    │
├─────────────────────────────┤
│ Internet                    │
│ IP, ICMP                    │
├─────────────────────────────┤
│ Network Access              │
│ Ethernet, Wi-Fi             │
└─────────────────────────────┘
```

Students don't need to memorize everything initially.

Understand the basic flow:

```text
Application
     ↓
TCP / UDP
     ↓
IP
     ↓
Ethernet / Wi-Fi
     ↓
Network
```

---

# 14. Networking in Cloud Computing

This is where **Networking becomes extremely important for AWS, Azure and GCP**.

The cloud providers have their own virtual networking services:

| Concept                            | AWS                            | Azure                                      | GCP                                               |
| ---------------------------------- | ------------------------------ | ------------------------------------------ | ------------------------------------------------- |
| Virtual Network                    | VPC                            | Virtual Network (VNet)                     | VPC                                               |
| Subnet                             | Subnet                         | Subnet                                     | Subnet                                            |
| Internet Gateway / Internet access | Internet Gateway               | Internet connectivity via Azure networking | Default/Custom routes & internet gateway behavior |
| Firewall                           | Security Groups / Network ACLs | NSG / Azure Firewall                       | VPC Firewall Rules / Cloud NGFW                   |
| Load Balancer                      | Elastic Load Balancing         | Azure Load Balancer / Application Gateway  | Cloud Load Balancing                              |
| DNS                                | Route 53                       | Azure DNS                                  | Cloud DNS                                         |

The terminology differs, but the **networking concepts are very similar**.

---

# 15. Basic Cloud Networking Architecture

```text
                         INTERNET
                             │
                             ↓
                       Public Network
                             │
                    ┌────────┴────────┐
                    │                 │
              Public Subnet      Public Subnet
                    │                 │
                 Web VM            Web VM
                    │                 │
                    └────────┬────────┘
                             ↓
                       Private Subnet
                             │
                        Backend/API
                             │
                             ↓
                       Database
```

### Important concept

A common cloud architecture separates resources into:

```text
Internet-facing
      ↓
Public Subnet
      ↓
Application Tier
      ↓
Private Subnet
      ↓
Database
```

This separation improves **security, control and architecture design**.

---

# 16. Networking Learning Roadmap

For **Module 3 — Networking**, I recommend teaching students in this sequence:

```text
MODULE 3 — NETWORKING
        │
        ├── 1. What is Network?
        ├── 2. What is Networking?
        ├── 3. Network Components
        │      ├── Switch
        │      ├── Router
        │      ├── Firewall
        │      └── Load Balancer
        │
        ├── 4. Network Types
        │      ├── LAN
        │      ├── WAN
        │      └── Internet
        │
        ├── 5. IP Address
        │      ├── IPv4
        │      ├── Public IP
        │      └── Private IP
        │
        ├── 6. Subnet
        ├── 7. CIDR
        ├── 8. Ports
        ├── 9. Protocols
        │      ├── TCP
        │      ├── UDP
        │      ├── HTTP/HTTPS
        │      ├── SSH
        │      └── DNS
        │
        ├── 10. Routing
        ├── 11. NAT
        ├── 12. DNS
        ├── 13. Firewall
        │
        └── 14. Cloud Networking
               ├── AWS VPC
               ├── Azure VNet
               └── GCP VPC
```

