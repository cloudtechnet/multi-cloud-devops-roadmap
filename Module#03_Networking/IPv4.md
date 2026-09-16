# Module 3 — Networking

# IPv4 — Internet Protocol Version 4

IPv4 is one of the most important concepts students need to understand before learning **AWS VPC, Azure VNet, GCP VPC, Subnets, CIDR, Routing, Security Groups and Firewalls**.

---

## 1. What is IPv4?

**IPv4 (Internet Protocol Version 4)** is a network-layer protocol used to identify devices/interfaces and route packets across IP networks.

An IPv4 address is **32 bits** long.

It is normally written as **four decimal numbers separated by dots**, called **octets**.

Example:

```text
192.168.1.10
```

Each octet represents **8 bits**:

```text
192       168       1        10
 │         │        │         │
8 bits    8 bits   8 bits    8 bits

        Total = 32 bits
```

Therefore:

> **IPv4 = 32 bits = 4 octets × 8 bits**

---

# 2. IPv4 Structure

An IPv4 address has 32 binary bits:

```text
  8 bits       8 bits       8 bits       8 bits
┌──────────┬──────────┬──────────┬──────────┐
│  Octet 1 │  Octet 2 │  Octet 3 │  Octet 4 │
└──────────┴──────────┴──────────┴──────────┘
     192        168          1          10
```

Example:

```text
192.168.1.10
```

Binary representation:

```text
192 = 11000000
168 = 10101000
  1 = 00000001
 10 = 00001010

IPv4:

11000000.10101000.00000001.00001010
```

Total:

```text
8 + 8 + 8 + 8 = 32 bits
```

---

# 3. What is a Bit?

A **bit** can have only two values:

```text
0
1
```

IPv4 uses binary internally.

For example:

```text
Decimal       Binary
---------------------
0             00000000
1             00000001
2             00000010
3             00000011
...
255           11111111
```

Because an octet has 8 bits:

```text
2⁸ = 256
```

Therefore each IPv4 octet can contain:

```text
0 – 255
```

So a valid IPv4 address looks like:

```text
10.20.30.40
192.168.1.10
8.8.8.8
172.16.50.100
```

An address such as:

```text
192.168.1.300
```

is **not a valid IPv4 address**, because an octet cannot exceed 255.

---

# 4. Why 32 Bits?

IPv4 was designed using a **32-bit address space**.

Number of possible combinations:

```text
2³²
```

Which equals:

```text
4,294,967,296
```

So IPv4 provides approximately:

> **4.29 billion possible address values**

However, not all of these are available for assignment to ordinary hosts because some ranges are reserved for special purposes, and historical allocation methods were not fully efficient.

---

# 5. IPv4 Address Representation

There are two important representations.

### Human-readable

```text
192.168.1.10
```

### Machine/binary representation

```text
11000000.10101000.00000001.00001010
```

Networking engineers frequently convert between these representations when working with:

* Subnets
* CIDR
* Network addresses
* Broadcast addresses
* Routing
* IP ranges

---

# 6. IPv4 Address = Network + Host

One of the most important concepts:

An IPv4 address can be divided into:

```text
┌─────────────────────┬────────────────────┐
│     Network Part    │     Host Part      │
└─────────────────────┴────────────────────┘
```

For example:

```text
192.168.1.10/24
```

With `/24`:

```text
Network                 Host
<---------------->      <-------->
192.168.1               .10
```

So:

```text
Network = 192.168.1.0
Host    = 10
```

The `/24` is called the **CIDR prefix length**.

We'll cover CIDR in detail separately.

---

# 7. IPv4 Address Classes — Historical Classification

Before CIDR became standard, IPv4 addresses were traditionally divided into **Class A, B, C, D and E**.

```text
IPv4 Classes

A → Large networks
B → Medium networks
C → Small networks
D → Multicast
E → Experimental / Reserved
```

This is called **classful networking**.

---

## Class A

First octet:

```text
1 – 126
```

Historical default subnet mask:

```text
255.0.0.0
```

CIDR:

```text
/8
```

Structure:

```text
┌────────┬───────────────────────────────┐
│Network │             Host              │
│ 8 bits │            24 bits            │
└────────┴───────────────────────────────┘
```

Example:

```text
10.0.0.1
```

Class A could support very large networks.

---

## Class B

First octet:

```text
128 – 191
```

Default mask:

```text
255.255.0.0
```

CIDR:

```text
/16
```

Structure:

```text
┌────────────────┬───────────────────────┐
│    Network     │         Host          │
│    16 bits     │        16 bits        │
└────────────────┴───────────────────────┘
```

Example:

```text
172.16.1.10
```

---

## Class C

First octet:

```text
192 – 223
```

Default mask:

```text
255.255.255.0
```

CIDR:

```text
/24
```

Structure:

```text
┌──────────────────────────┬─────────────┐
│         Network          │    Host     │
│          24 bits         │   8 bits    │
└──────────────────────────┴─────────────┘
```

Example:

```text
192.168.1.10
```

---

## Class D

First octet:

```text
224 – 239
```

Used for:

> **Multicast**

Example:

```text
224.0.0.1
```

It is not used for normal host addressing in the way Class A/B/C historically were.

---

## Class E

First octet:

```text
240 – 255
```

Historically designated for:

> **Experimental / reserved purposes**

It is not used as ordinary unicast host addressing.

---

# 8. Classful IPv4 Summary

| Class | First Octet | Default Mask  | CIDR | Historical Purpose    |
| ----- | ----------: | ------------- | ---- | --------------------- |
| A     |       1–126 | 255.0.0.0     | /8   | Large networks        |
| B     |     128–191 | 255.255.0.0   | /16  | Medium networks       |
| C     |     192–223 | 255.255.255.0 | /24  | Small networks        |
| D     |     224–239 | N/A           | N/A  | Multicast             |
| E     |     240–255 | N/A           | N/A  | Experimental/reserved |

### Important exception

`127.0.0.0/8` is reserved for **loopback**.

For example:

```text
127.0.0.1
```

is commonly called:

> **localhost**

---

# 9. Why Was Classful Networking Replaced?

Classful networking had a major problem:

### IP address wastage

Suppose an organization needed approximately:

```text
500 IP addresses
```

A Class C network provided only:

```text
256 addresses
```

Not enough.

A Class B network provided:

```text
65,536 addresses
```

Far more than required.

So:

```text
Required:       ~500
Class C:         256  ❌
Class B:      65,536  ❌ huge allocation
```

This caused inefficient use of IPv4 address space.

---

# 10. CIDR — Classless Inter-Domain Routing

CIDR was introduced to make IP allocation and routing more flexible.

Instead of relying on:

```text
Class A
Class B
Class C
```

we use a **prefix length**.

Examples:

```text
10.0.0.0/8
10.0.0.0/16
10.0.0.0/24
10.0.0.0/28
```

The `/number` tells us how many bits represent the network prefix.

For example:

```text
192.168.1.0/24
```

means:

```text
Network = 24 bits
Host    = 8 bits
```

CIDR is fundamental to modern cloud networking.

---

# 11. Public IPv4 vs Private IPv4

Another very important classification is:

```text
IPv4
 │
 ├── Public IPv4
 │
 └── Private IPv4
```

---

## Private IPv4 Addresses

Private IPv4 ranges are defined by RFC 1918.

### Range 1

```text
10.0.0.0/8
```

Range:

```text
10.0.0.0 – 10.255.255.255
```

### Range 2

```text
172.16.0.0/12
```

Range:

```text
172.16.0.0 – 172.31.255.255
```

### Range 3

```text
192.168.0.0/16
```

Range:

```text
192.168.0.0 – 192.168.255.255
```

These addresses are intended for **private networks** and are not globally routed on the public Internet.

Example:

```text
Office Network

PC1 → 192.168.1.10
PC2 → 192.168.1.11
PC3 → 192.168.1.12
```

---

# 12. Public IPv4

A public IPv4 address is globally unique within the Internet addressing system and can be used for Internet routing, subject to routing and security controls.

Example:

```text
Client
   │
Internet
   │
Public IPv4
   │
Web Server
```

Cloud platforms commonly provide public IPv4 addresses for resources that need Internet connectivity.

---

# 13. Static vs Dynamic IPv4

IPv4 addresses can also be classified according to how they are assigned.

### Static IP

The address remains assigned to a device/resource until explicitly changed or released.

Example:

```text
Server → 10.0.1.10
```

Useful for systems where a stable address is required.

### Dynamic IP

The address can be assigned automatically and may change.

A common mechanism is:

> **DHCP — Dynamic Host Configuration Protocol**

Example:

```text
Laptop
   ↓
DHCP Server
   ↓
192.168.1.25
```

---

# 14. Special IPv4 Addresses

Several IPv4 ranges/addresses have special meanings.

### Loopback

```text
127.0.0.0/8
```

Common example:

```text
127.0.0.1
```

Used by a machine to communicate with itself.

```text
Application
    ↓
127.0.0.1
    ↓
Same machine
```

---

### Unspecified Address

```text
0.0.0.0
```

It can mean "this host" or "unspecified address," depending on context.

For example, a server application listening on:

```text
0.0.0.0
```

typically means it accepts connections on all available local IPv4 interfaces.

---

### Limited Broadcast

```text
255.255.255.255
```

Used for broadcast within the local network context.

---

### Link-local / APIPA

```text
169.254.0.0/16
```

Commonly associated with automatic link-local addressing when normal DHCP configuration is unavailable.

Example:

```text
169.254.10.20
```

---

# 15. Unicast, Broadcast and Multicast

IPv4 traffic can also be classified by **how packets are delivered**.

```text
IPv4 Traffic
     │
 ┌───┼────────┐
 ↓   ↓        ↓
Unicast Broadcast Multicast
```

### Unicast

One sender → One receiver.

```text
PC A ─────────→ Server
```

Example:

```text
Client → Web Server
```

---

### Broadcast

One sender → All applicable hosts on the local broadcast domain.

```text
        ┌→ PC1
Sender ─┼→ PC2
        ├→ PC3
        └→ PC4
```

IPv4 broadcast is generally limited to the local network and routers do not forward ordinary broadcasts between networks.

---

### Multicast

One sender → Multiple interested receivers.

```text
             ┌→ Client A
Server ──────┼→ Client B
             └→ Client C
```

IPv4 multicast uses:

```text
224.0.0.0 – 239.255.255.255
```

---

# 16. IPv4 Packet Structure

An IPv4 address is different from an **IPv4 packet**.

Students often confuse these.

### IPv4 Address

Identifies the source/destination interface:

```text
192.168.1.10
```

### IPv4 Packet

Carries data through the network.

Simplified structure:

```text
┌─────────────────────────────────────┐
│           IPv4 Header               │
├─────────────────────────────────────┤
│ Source IP Address                   │
├─────────────────────────────────────┤
│ Destination IP Address              │
├─────────────────────────────────────┤
│ Other Header Information             │
├─────────────────────────────────────┤
│              Payload                │
│         Application Data            │
└─────────────────────────────────────┘
```

Important IPv4 header fields include:

* Version
* IHL
* DSCP/ECN
* Total Length
* Identification
* Flags
* Fragment Offset
* TTL
* Protocol
* Header Checksum
* Source Address
* Destination Address
* Options

---

# 17. TTL — Time To Live

IPv4 packets contain a **TTL (Time To Live)** field.

It prevents packets from circulating indefinitely because of routing loops.

Conceptually:

```text
Client
 ↓ TTL=64
Router
 ↓ TTL=63
Router
 ↓ TTL=62
Router
 ↓ TTL=61
Server
```

Each router forwarding the packet decrements the TTL.

If TTL reaches zero, the packet is discarded.

---

# 18. IPv4 Protocol Field

The IPv4 header contains a **Protocol** field that indicates which transport-layer protocol should receive the payload.

Examples:

```text
IPv4
 │
 ├── TCP
 ├── UDP
 └── ICMP
```

For example:

```text
HTTP/HTTPS
     ↓
    TCP
     ↓
    IPv4
     ↓
 Ethernet/Wi-Fi
```

---

# 19. IPv4 Communication Example

Let's follow a request:

```text
User
192.168.1.10
       │
       │ HTTPS
       ↓
Router
       │
       ↓
Internet
       │
       ↓
Web Server
203.x.x.x
```

The packet contains information such as:

```text
Source IP:
192.168.1.10

Destination IP:
203.x.x.x

Protocol:
TCP
```

In a typical home/office setup, **NAT** may translate the private source address to a public address before the packet reaches the Internet.

---

# 20. IPv4 History — Important Timeline

IPv4 has a long history.

### 1960s–1970s — ARPANET and Internetworking

Research networks such as **ARPANET** helped develop the foundations of packet-switched networking.

The need emerged for different networks to communicate with each other.

---

### 1974 — TCP/IP Concept

Researchers including **Vint Cerf and Bob Kahn** published influential work describing a protocol architecture for communication between interconnected networks.

This became a major foundation for modern Internet networking.

---

### 1981 — IPv4 Specification

The Internet Protocol was formally specified in **RFC 791** in 1981.

IPv4 uses:

```text
32-bit addresses
```

---

### 1983 — TCP/IP Becomes Core ARPANET Protocol Suite

On **January 1, 1983**, ARPANET transitioned to TCP/IP.

This date is commonly associated with the beginning of the modern Internet architecture.

---

### 1980s–1990s — Rapid Internet Growth

The Internet expanded significantly.

More:

* Universities
* Organizations
* Businesses
* Internet Service Providers
* Personal computers

started connecting.

IPv4 addresses began becoming a valuable resource.

---

### 1990s — CIDR

**CIDR (Classless Inter-Domain Routing)** was introduced to improve address allocation and routing efficiency.

Instead of relying on:

```text
Class A
Class B
Class C
```

networks could use variable prefix lengths:

```text
/8
/16
/20
/24
/27
/28
...
```

---

### 1990s — NAT

**Network Address Translation (NAT)** became widely used.

NAT allowed many private devices to share fewer public IPv4 addresses.

```text
             Internet
                 │
            Public IP
                 │
               NAT
                 │
       ┌─────────┼─────────┐
       ↓         ↓         ↓
 192.168.1.10  .11       .12
```

This significantly reduced pressure on public IPv4 addresses.

---

### 1990s — IPv6 Development

Because IPv4 has a 32-bit address space, a much larger addressing system was needed for the future.

IPv6 was developed with:

```text
128-bit addresses
```

Example:

```text
2001:db8::1
```

---

### 2011 — IANA IPv4 Pool Exhaustion

The **Internet Assigned Numbers Authority (IANA)** allocated its remaining general IPv4 address blocks to the Regional Internet Registries in 2011.

Regional registries subsequently reached their own IPv4 exhaustion milestones at different times.

---

### Today

IPv4 remains extremely widely deployed.

At the same time, the Internet increasingly uses:

```text
IPv4 + IPv6
```

through mechanisms such as:

* Dual-stack
* NAT
* IPv6 transition technologies

---

# 21. IPv4 vs IPv6

| Feature            | IPv4            | IPv6                                            |
| ------------------ | --------------- | ----------------------------------------------- |
| Address size       | 32-bit          | 128-bit                                         |
| Example            | 192.168.1.10    | 2001:db8::1                                     |
| Address notation   | Decimal + dots  | Hexadecimal + colons                            |
| Address space      | ~4.29 billion   | 2¹²⁸                                            |
| NAT                | Widely used     | Generally not required for address conservation |
| Broadcast          | Supported       | No broadcast; multicast is used                 |
| Header             | Variable length | Fixed base header                               |
| Current deployment | Very widespread | Increasing                                      |

---

# 22. IPv4 in AWS, Azure and GCP

This is particularly important for your **Multi-Cloud DevOps course**.

The same fundamental IPv4 concepts appear across all three clouds.

```text
                 IPv4 Networking
                       │
          ┌────────────┼────────────┐
          ↓            ↓            ↓
         AWS          Azure         GCP
          │            │            │
         VPC          VNet          VPC
          │            │            │
       Subnets       Subnets      Subnets
          │            │            │
       IPv4 CIDR     IPv4 CIDR    IPv4 CIDR
          │            │            │
       Routing       Routing      Routing
          │            │            │
       Firewall      NSG/etc.     Firewall
```

For example, you may design:

```text
10.0.0.0/16
```

as the overall private network and divide it into smaller subnets:

```text
10.0.1.0/24  → Web
10.0.2.0/24  → Application
10.0.3.0/24  → Database
```

The same **IP → CIDR → subnet → route → security** concepts then transfer across AWS, Azure and GCP.

---

# 23. Important IPv4 Concepts Students Must Remember

```text
                    IPv4
                      │
       ┌──────────────┼──────────────┐
       ↓              ↓              ↓
   32 Bits        Address Types    Traffic
       │              │              │
   4 Octets      Public/Private   Unicast
       │          Static/Dynamic   Broadcast
       ↓                         Multicast
   Network +
     Host
       │
       ↓
     CIDR
       │
       ↓
    Subnet
       │
       ↓
    Routing
```

### Quick memory points

**IPv4**

```text
32 bits
4 octets
8 bits/octet
0–255/octet
2³² ≈ 4.29 billion addresses
```

**Private ranges**

```text
10.0.0.0/8
172.16.0.0/12
192.168.0.0/16
```

**Special ranges**

```text
127.0.0.0/8       → Loopback
169.254.0.0/16    → Link-local
224.0.0.0/4       → Multicast
240.0.0.0/4       → Historically Class E / reserved
```

**Historical classes**

```text
A → 1–126
B → 128–191
C → 192–223
D → 224–239
E → 240–255
```
