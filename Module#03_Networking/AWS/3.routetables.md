# AWS Route Tables — Complete Explanation

An **AWS Route Table** is like a **traffic direction table** for a VPC.

It tells AWS:

> **“When traffic is going to this destination, where should AWS send it?”**

In simple terms:

**Route Table = Traffic Rules / Routing Instructions**

---

## 1. Where does a Route Table fit?

A typical AWS network looks like this:

```text
                         INTERNET
                            │
                            ▼
                    ┌──────────────┐
                    │ Internet     │
                    │ Gateway (IGW)│
                    └──────┬───────┘
                           │
                    ┌──────▼───────┐
                    │     VPC      │
                    │ 10.0.0.0/16  │
                    │              │
                    │ Route Table  │
                    │              │
                    └──────┬───────┘
                           │
              ┌────────────┴────────────┐
              │                         │
       ┌──────▼───────┐         ┌──────▼───────┐
       │ Public       │         │ Private      │
       │ Subnet       │         │ Subnet       │
       │10.0.1.0/24   │         │10.0.2.0/24   │
       └──────────────┘         └──────┬────────┘
                                      │
                                  ┌───▼───┐
                                  │ NAT   │
                                  │Gateway│
                                  └───┬───┘
                                      │
                                      ▼
                                   Internet
```

The **Route Table decides the next destination for network traffic**.

---

# 2. Simple Real-World Example

Imagine you are driving in Hyderabad.

You want to go to:

```text
Hyderabad → Bangalore
```

A traffic system needs to know:

```text
Destination       Next Direction
--------------------------------
Bangalore         ORR → NH44
```

Similarly, AWS needs routing rules:

```text
Destination       Target
-----------------------------
10.0.0.0/16       local
0.0.0.0/0         Internet Gateway
```

AWS reads the destination IP and selects the appropriate route.

---

# 3. Example VPC

Let's create:

```text
VPC CIDR
10.0.0.0/16
```

This gives us the VPC network:

```text
10.0.0.0
     │
     ├── 10.0.0.0/24
     ├── 10.0.1.0/24
     ├── 10.0.2.0/24
     ├── 10.0.3.0/24
     └── ...
```

For example:

```text
VPC
10.0.0.0/16
       │
       ├── Public Subnet
       │   10.0.1.0/24
       │
       └── Private Subnet
           10.0.2.0/24
```

---

# 4. What is inside a Route Table?

A Route Table contains **routes**.

Each route generally has:

```text
Destination → Target
```

For example:

```text
10.0.0.0/16 → local
0.0.0.0/0   → Internet Gateway
```

### Destination

Destination means:

> Where is the traffic trying to go?

### Target

Target means:

> Where should AWS send that traffic next?

---

# 5. Important Route Table Example

Suppose we have:

```text
VPC: 10.0.0.0/16

Public Subnet:
10.0.1.0/24
```

The route table might look like:

| Destination | Target     |
| ----------- | ---------- |
| 10.0.0.0/16 | local      |
| 0.0.0.0/0   | igw-123456 |

Meaning:

### Route 1

```text
10.0.0.0/16 → local
```

Traffic going inside the VPC stays inside the VPC.

For example:

```text
EC2-1
10.0.1.10
   │
   │
   ▼
EC2-2
10.0.2.10
```

Both belong to:

```text
10.0.0.0/16
```

So AWS uses:

```text
10.0.0.0/16 → local
```

---

# 6. What is `0.0.0.0/0`?

This is **very important**.

```text
0.0.0.0/0
```

means:

> **Any IPv4 destination**

It is commonly called the **default route**.

For example:

```text
0.0.0.0/0 → Internet Gateway
```

means:

> If the destination does not match another more specific route, send the traffic to the Internet Gateway.

---

# 7. Public Subnet Route Table

Let's build a public subnet.

```text
VPC
10.0.0.0/16
        │
        ▼
Public Subnet
10.0.1.0/24
        │
        ▼
Route Table
```

Routes:

```text
Destination       Target
--------------------------------
10.0.0.0/16       local
0.0.0.0/0         Internet Gateway
```

Architecture:

```text
                     INTERNET
                         ▲
                         │
                  0.0.0.0/0
                         │
                         ▼
               ┌─────────────────┐
               │ Internet Gateway│
               └────────┬────────┘
                        │
                        ▼
              ┌──────────────────┐
              │   Route Table    │
              │                  │
              │ 10.0.0.0/16 local│
              │ 0.0.0.0/0 → IGW  │
              └────────┬─────────┘
                       │
                       ▼
                Public Subnet
                 10.0.1.0/24
                       │
                       ▼
                   EC2 Server
                  10.0.1.10
```

This allows a properly configured EC2 instance in the public subnet to communicate with the internet.

---

# 8. Why is it called a Public Subnet?

A subnet is considered **public** when its associated route table has a route to an **Internet Gateway**.

For example:

```text
0.0.0.0/0 → Internet Gateway
```

But remember:

**Route table alone does not make an EC2 instance internet-accessible.**

The instance also needs appropriate networking configuration, such as:

* Public IPv4 address or another valid internet path
* Security Group rules
* Network ACL rules
* Internet Gateway attached to the VPC

---

# 9. Private Subnet Route Table

Now let's create:

```text
Private Subnet
10.0.2.0/24
```

We don't want the private subnet to directly use the Internet Gateway.

Instead, we can use a **NAT Gateway** for outbound internet access.

```text
Private EC2
10.0.2.10
     │
     ▼
Private Route Table
     │
     │ 0.0.0.0/0
     ▼
 NAT Gateway
     │
     ▼
Internet Gateway
     │
     ▼
 Internet
```

Private route table:

| Destination | Target      |
| ----------- | ----------- |
| 10.0.0.0/16 | local       |
| 0.0.0.0/0   | NAT Gateway |

---

# 10. Public vs Private Route Table

### Public Route Table

```text
10.0.0.0/16 → local
0.0.0.0/0   → Internet Gateway
```

Architecture:

```text
EC2
 │
 ▼
Route Table
 │
 ▼
Internet Gateway
 │
 ▼
Internet
```

### Private Route Table

```text
10.0.0.0/16 → local
0.0.0.0/0   → NAT Gateway
```

Architecture:

```text
Private EC2
     │
     ▼
Route Table
     │
     ▼
 NAT Gateway
     │
     ▼
Internet Gateway
     │
     ▼
 Internet
```

---

# 11. How does AWS choose a route?

This is an important interview question.

Suppose your route table contains:

```text
10.0.0.0/16 → local
10.0.1.0/24 → something
0.0.0.0/0   → IGW
```

Traffic destination:

```text
10.0.1.50
```

Which route will AWS choose?

It chooses:

```text
10.0.1.0/24
```

because it is **more specific**.

This concept is called **Longest Prefix Match**.

### Example

```text
Destination IP: 10.0.1.50
```

Matches:

```text
10.0.0.0/16
10.0.1.0/24
0.0.0.0/0
```

AWS chooses:

```text
10.0.1.0/24
```

because `/24` is more specific than `/16`, and `/16` is more specific than `/0`.

---

# 12. What is `local` route?

When you create a VPC:

```text
10.0.0.0/16
```

AWS automatically creates a local route similar to:

```text
10.0.0.0/16 → local
```

This allows communication between subnets within the VPC, subject to other network controls.

For example:

```text
10.0.1.10
   │
   │
   ▼
10.0.2.10
```

Both are inside:

```text
10.0.0.0/16
```

Therefore:

```text
10.0.0.0/16 → local
```

handles the routing.

---

# 13. Route Table Association

A Route Table is associated with one or more subnets.

Example:

```text
VPC
10.0.0.0/16
       │
       ├───────────────┐
       │               │
       ▼               ▼
Public Subnet      Private Subnet
10.0.1.0/24        10.0.2.0/24
       │               │
       ▼               ▼
Public RT          Private RT
```

### Public RT

```text
10.0.0.0/16 → local
0.0.0.0/0   → IGW
```

### Private RT

```text
10.0.0.0/16 → local
0.0.0.0/0   → NAT Gateway
```

So:

```text
Public Subnet
      ↓
Public Route Table
      ↓
Internet Gateway
```

and:

```text
Private Subnet
      ↓
Private Route Table
      ↓
NAT Gateway
```

---

# 14. One Route Table can be used by multiple Subnets

For example:

```text
             Public Route Table
                    │
          ┌─────────┼─────────┐
          ▼         ▼         ▼
       Subnet-A  Subnet-B  Subnet-C
```

All three subnets can use the same routing rules.

Similarly:

```text
            Private Route Table
                    │
             ┌──────┼──────┐
             ▼      ▼      ▼
          Subnet-D Subnet-E Subnet-F
```

---

# 15. Main Route Table

Every VPC has a **main route table**.

When a subnet is not explicitly associated with another route table, it uses the VPC's main route table.

Example:

```text
VPC
 │
 ├── Main Route Table
 │
 ├── Public Route Table
 │
 └── Private Route Table
```

You can explicitly associate a subnet with a custom route table.

---

# 16. Complete AWS Example

Let's create a real-world architecture.

### VPC

```text
10.0.0.0/16
```

### Subnets

```text
Public Subnet
10.0.1.0/24

Private App Subnet
10.0.2.0/24

Private DB Subnet
10.0.3.0/24
```

Architecture:

```text
                         INTERNET
                            │
                            ▼
                     Internet Gateway
                            │
              ┌─────────────┴─────────────┐
              │                           │
              ▼                           │
       Public Route Table                 │
       10.0.0.0/16 → local                │
       0.0.0.0/0 → IGW                    │
              │                            │
              ▼                            │
       Public Subnet                       │
       10.0.1.0/24                         │
              │                            │
              ▼                            │
          Load Balancer                    │
              │                            │
              ▼                            │
       Private App Subnet                  │
       10.0.2.0/24                         │
              │                            │
              ▼                            │
          App Servers                      │
              │                            │
              ▼                            │
       Private DB Subnet                   │
       10.0.3.0/24                         │
              │                            │
              ▼                            │
          Database                         │
                                           │
       NAT Gateway ────────────────────────┘
```

---

# 17. Important Route Table Targets

A Route Table can route traffic to different AWS networking components depending on the architecture.

Common targets include:

| Target                         | Purpose                               |
| ------------------------------ | ------------------------------------- |
| `local`                        | Communication inside VPC              |
| Internet Gateway               | Internet access                       |
| NAT Gateway                    | Outbound internet from private subnet |
| Transit Gateway                | Connect multiple VPCs/networks        |
| VPC Peering                    | Connect two VPCs                      |
| Virtual Private Gateway        | VPN connectivity                      |
| Network Firewall               | Network security inspection           |
| Gateway Load Balancer endpoint | Security appliance traffic            |

---

# 18. Route Table vs Security Group

Students often confuse these.

### Route Table

Answers:

> **Where should traffic go?**

```text
Destination → Target
```

### Security Group

Answers:

> **Is this traffic allowed?**

Example:

```text
TCP 443 → Allow
TCP 22  → Allow
```

So remember:

```text
Route Table
     ↓
Where should traffic go?

Security Group
     ↓
Is traffic allowed?
```

---

# 19. Route Table vs NACL

Another common interview question.

| Feature      | Route Table        | NACL                   |
| ------------ | ------------------ | ---------------------- |
| Main purpose | Routing            | Traffic filtering      |
| Decides      | Where traffic goes | Allow/Deny traffic     |
| Works with   | Routes             | IP/Port/Protocol rules |
| Example      | `0.0.0.0/0 → IGW`  | `TCP 443 → ALLOW`      |

Simple memory trick:

```text
Route Table = Direction
Security Group = Permission
NACL = Network-level filtering
```

---

# 20. Interview Answer

If an interviewer asks:

**"What is a Route Table in AWS?"**

You can answer:

> An AWS Route Table contains routing rules that determine where network traffic should be sent. Each route has a destination CIDR and a target, such as a local VPC route, Internet Gateway, NAT Gateway, Transit Gateway, or VPC peering connection. For example, in a VPC with `10.0.0.0/16`, a public subnet route table can have `10.0.0.0/16 → local` and `0.0.0.0/0 → Internet Gateway`. AWS uses the most specific matching route, known as longest prefix match, when selecting a route.
