# AWS Subnet — End-to-End Explanation

Let's understand **AWS Subnet** from the basics using this example:

> **VPC CIDR: `10.0.0.0/16`**

---

## 1. What is a Subnet?

A **Subnet (Subnetwork)** is a smaller network created inside an AWS VPC.

Think of the **VPC as a large building** and **subnets as different rooms inside that building**.

```text
                    AWS REGION
                       │
                       ▼
              ┌───────────────────┐
              │       VPC         │
              │   10.0.0.0/16     │
              │                   │
              │  ┌─────────────┐  │
              │  │   Subnet 1  │  │
              │  │10.0.1.0/24  │  │
              │  └─────────────┘  │
              │                   │
              │  ┌─────────────┐  │
              │  │   Subnet 2  │  │
              │  │10.0.2.0/24  │  │
              │  └─────────────┘  │
              │                   │
              │  ┌─────────────┐  │
              │  │   Subnet 3  │  │
              │  │10.0.3.0/24  │  │
              │  └─────────────┘  │
              └───────────────────┘
```

A subnet **cannot exist outside a VPC**.

---

# 2. Why Do We Need Subnets?

Suppose your VPC has:

```text
10.0.0.0/16
```

This gives you a large private IP address range.

Instead of putting every resource into one network, we divide it into smaller networks.

For example:

```text
VPC
10.0.0.0/16
      │
      ├── Public Subnet
      │   10.0.1.0/24
      │
      ├── Private Application Subnet
      │   10.0.2.0/24
      │
      └── Private Database Subnet
          10.0.3.0/24
```

This helps us organize resources and control network traffic.

---

# 3. Understanding VPC CIDR

Our VPC:

```text
10.0.0.0/16
```

Let's understand what `/16` means.

IPv4 has **32 bits**.

```text
10.0.0.0

10       .0       .0       .0
│         │        │        │
8 bits   8 bits   8 bits   8 bits

Total = 32 bits
```

With:

```text
10.0.0.0/16
```

The first **16 bits are the network portion**.

```text
10.0        .       0.0
Network             Host
<----16---->        <----16---->
```

Therefore:

```text
VPC CIDR = 10.0.0.0/16
```

provides:

```text
65,536 IPv4 addresses
```

Because:

```text
2^(32-16)
= 2^16
= 65,536
```

---

# 4. Creating Subnets From the VPC

Our VPC is:

```text
10.0.0.0/16
```

We can divide it into smaller `/24` networks.

For example:

```text
                 VPC
             10.0.0.0/16
                  │
        ┌─────────┼─────────┐
        │         │         │
        ▼         ▼         ▼
   10.0.1.0/24 10.0.2.0/24 10.0.3.0/24
```

Each `/24` contains:

```text
256 IP addresses
```

because:

```text
2^(32-24)
= 2^8
= 256
```

---

# 5. Example AWS Architecture

Let's build a simple 3-tier application.

```text
                         INTERNET
                             │
                             ▼
                    ┌────────────────┐
                    │ Internet Gateway│
                    └───────┬────────┘
                            │
                VPC: 10.0.0.0/16
                            │
          ┌─────────────────┴─────────────────┐
          │                                   │
          ▼                                   ▼
 ┌──────────────────┐                ┌──────────────────┐
 │  Public Subnet   │                │  Public Subnet   │
 │  10.0.1.0/24     │                │  10.0.2.0/24     │
 │                  │                │                  │
 │  Web Server      │                │  Load Balancer   │
 └──────────────────┘                └──────────────────┘
          │
          ▼
 ┌──────────────────┐
 │ Private Subnet   │
 │ 10.0.3.0/24      │
 │                  │
 │ Application      │
 │ Server           │
 └──────────────────┘
          │
          ▼
 ┌──────────────────┐
 │ Private Subnet   │
 │ 10.0.4.0/24      │
 │                  │
 │ RDS Database     │
 └──────────────────┘
```

---

# 6. Public vs Private Subnet

This is an important concept.

### Public Subnet

A subnet is considered **public** when its routing configuration provides a path to an **Internet Gateway**.

Example:

```text
10.0.1.0/24
```

Resources such as:

* Web servers
* Load balancers
* Bastion hosts

may be placed there, depending on the architecture.

### Private Subnet

A subnet without a direct route to an Internet Gateway is commonly used as a **private subnet**.

Example:

```text
10.0.3.0/24
```

Resources such as:

* Application servers
* Databases
* Internal services

can be placed there.

**Important:** Public/private status is primarily determined by **routing**, not simply by the subnet's IP address.

---

# 7. Subnet CIDR Example

Let's take:

```text
VPC = 10.0.0.0/16
```

We create:

| Subnet   | CIDR          | Purpose     |
| -------- | ------------- | ----------- |
| Subnet-1 | `10.0.1.0/24` | Web         |
| Subnet-2 | `10.0.2.0/24` | Web         |
| Subnet-3 | `10.0.3.0/24` | Application |
| Subnet-4 | `10.0.4.0/24` | Database    |

All of these belong to:

```text
10.0.0.0/16
```

---

# 8. How `/16` Becomes `/24`

This is one of the most important subnetting concepts.

VPC:

```text
10.0.0.0/16
```

We use `/24` for each subnet.

```text
10.0.0.0/16
       │
       ├── 10.0.0.0/24
       ├── 10.0.1.0/24
       ├── 10.0.2.0/24
       ├── 10.0.3.0/24
       ├── 10.0.4.0/24
       ├── 10.0.5.0/24
       │
       └── ...
```

There are:

```text
2^(24-16)
= 2^8
= 256
```

possible `/24` subnet blocks inside the `/16` range.

---

# 9. How Many IPs Does a `/24` Have?

Example:

```text
10.0.1.0/24
```

Range:

```text
10.0.1.0
       ↓
10.0.1.255
```

Total:

```text
256 IP addresses
```

However, in an AWS subnet, **5 IP addresses are reserved by AWS**.

So:

```text
256 - 5
= 251 usable IPv4 addresses
```

For example:

```text
10.0.1.0/24

10.0.1.0       Reserved
10.0.1.1       Reserved
10.0.1.2       Reserved
10.0.1.3       Reserved
10.0.1.4       Reserved
10.0.1.5
10.0.1.6
10.0.1.7
...
10.0.1.254
10.0.1.255
```

AWS reserves five IPv4 addresses in each subnet, so a `/24` has **251 usable IPv4 addresses**.

---

# 10. Why Does AWS Reserve 5 IP Addresses?

For an IPv4 subnet, AWS reserves:

```text
Network address
VPC router
DNS
Future use
Broadcast address
```

For:

```text
10.0.1.0/24
```

AWS reserves:

```text
10.0.1.0
10.0.1.1
10.0.1.2
10.0.1.3
10.0.1.255
```

Therefore:

```text
256 - 5 = 251
```

usable addresses.

---

# 11. Subnet Must Belong to VPC CIDR

Suppose:

```text
VPC = 10.0.0.0/16
```

This is valid:

```text
10.0.1.0/24
10.0.2.0/24
10.0.100.0/24
10.0.200.0/24
```

But this is **not valid**:

```text
192.168.1.0/24
```

because:

```text
192.168.1.0/24
```

doesn't fall inside:

```text
10.0.0.0/16
```

Think of the VPC as the **parent network** and subnets as **child networks**.

---

# 12. Subnets and Availability Zones

A very important AWS concept:

> **Every subnet belongs to one Availability Zone.**

For example:

```text
Region: ap-south-1
          │
          ├── AZ: ap-south-1a
          │      ├── Public Subnet
          │      │   10.0.1.0/24
          │      └── Private Subnet
          │          10.0.3.0/24
          │
          └── AZ: ap-south-1b
                 ├── Public Subnet
                 │   10.0.2.0/24
                 └── Private Subnet
                     10.0.4.0/24
```

This allows us to design applications across multiple Availability Zones.

---

# 13. Why Use Multiple Subnets?

Suppose we have:

```text
Web Application
```

Instead of putting everything into one subnet:

```text
VPC
 │
 └── One subnet
      │
      ├── Web
      ├── Application
      └── Database
```

we can separate them:

```text
                  VPC
             10.0.0.0/16
                    │
       ┌────────────┼────────────┐
       │            │            │
       ▼            ▼            ▼
     WEB           APP           DB
10.0.1.0/24   10.0.3.0/24   10.0.5.0/24
```

This provides better:

* Network organization
* Security control
* Traffic management
* High availability design
* Resource isolation

---

# 14. Real-Time Example — E-Commerce Application

Let's use your **QuickKart/Zepto-style application** as an example.

VPC:

```text
10.0.0.0/16
```

Architecture:

```text
                         Internet
                            │
                            ▼
                    Internet Gateway
                            │
                    VPC 10.0.0.0/16
                            │
             ┌──────────────┴──────────────┐
             │                             │
             ▼                             ▼
       Public Subnet                 Public Subnet
       10.0.1.0/24                   10.0.2.0/24
       AZ-1                          AZ-2
             │                             │
             └──────────────┬──────────────┘
                            │
                            ▼
                    Load Balancer
                            │
                    ┌───────┴───────┐
                    ▼               ▼
             Private Subnet   Private Subnet
             10.0.3.0/24      10.0.4.0/24
             AZ-1             AZ-2
                    │               │
                    └───────┬───────┘
                            ▼
                      Application
                        Servers
                            │
                            ▼
                    Database Subnets
                    10.0.5.0/24
                    10.0.6.0/24
```

This gives us a clean multi-AZ architecture.

---

# 15. Important Difference: VPC vs Subnet

| VPC                   | Subnet                     |
| --------------------- | -------------------------- |
| Large virtual network | Smaller network inside VPC |
| Example `10.0.0.0/16` | Example `10.0.1.0/24`      |
| Regional              | Belongs to one AZ          |
| Contains subnets      | Contains resources         |
| Parent network        | Child network              |

Simple way to remember:

```text
AWS Region
    │
    ▼
   VPC
10.0.0.0/16
    │
    ├── Subnet
    │   10.0.1.0/24
    │
    ├── Subnet
    │   10.0.2.0/24
    │
    └── Subnet
        10.0.3.0/24
```

---

# 16. Important Rules to Remember

### Rule 1

A subnet must be inside a VPC CIDR.

```text
VPC:    10.0.0.0/16
Subnet: 10.0.1.0/24       ✅
```

### Rule 2

Subnets cannot overlap.

```text
10.0.1.0/24     ✅
10.0.2.0/24     ✅

10.0.1.0/24
10.0.1.0/25     ❌ Overlapping
```

### Rule 3

A subnet belongs to one Availability Zone.

```text
Subnet → AZ
```

### Rule 4

A subnet can be public or private based mainly on its routing.

### Rule 5

AWS reserves 5 IPv4 addresses in every subnet.

---

# 17. Final Picture to Explain to Students

```text
                         AWS REGION
                             │
              ┌──────────────┴──────────────┐
              │                             │
            AZ-1                           AZ-2
              │                             │
        ┌─────┴─────┐                 ┌─────┴─────┐
        │           │                 │           │
        ▼           ▼                 ▼           ▼
     Public      Private            Public      Private
     Subnet      Subnet             Subnet      Subnet
  10.0.1.0/24 10.0.3.0/24        10.0.2.0/24 10.0.4.0/24
        │           │                 │           │
       ALB        App               ALB          App
        │           │                 │           │
        └───────────┴───────┬─────────┴───────────┘
                            │
                            ▼
                         VPC
                     10.0.0.0/16
```

### One-line definition for students

> **A subnet is a smaller IP network created inside an AWS VPC, used to organize resources and control how they communicate within the AWS network.**

### Remember this hierarchy

```text
AWS Region
     ↓
Availability Zone
     ↓
VPC
     ↓
Subnet
     ↓
EC2 / Load Balancer / Application Resources
```

**Next concept to teach after Subnet:** **AWS Route Table**, because the route table explains **why one subnet becomes public and another remains private**.
