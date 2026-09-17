# Module 3 — Networking

# AWS VPC — Virtual Private Cloud

> **Scope:** This lesson explains **AWS VPC only**.
> We will **not** cover VPC Subnets, Route Tables, Security Groups, or NACLs here.

---

## 1. What is AWS VPC?

**Amazon VPC (Virtual Private Cloud)** is a logically isolated virtual network that you create inside AWS.

It gives you control over the networking environment in which your AWS resources run.

Simple definition:

> **VPC is your private network boundary inside AWS.**

For example, if you create EC2 instances, databases, or other resources that support VPC networking, they can be placed within your VPC environment.

---

# 2. Why Do We Need a VPC?

Imagine running an application directly on a huge public network.

```text
                 AWS
                  │
       ┌──────────┼──────────┐
       │          │          │
      App        App        DB
```

You need a way to create an isolated networking environment for your own application.

VPC provides that logical network boundary:

```text
                    AWS Cloud
                       │
             ┌─────────┴─────────┐
             │       Your VPC    │
             │                   │
             │   Your Network    │
             │                   │
             └───────────────────┘
```

You can then build your application networking within that VPC.

---

# 3. Real-World Analogy

Think about a large apartment complex.

```text
                 Apartment Complex
                        │
              ┌─────────┴─────────┐
              │                   │
          Apartment A         Apartment B
              │                   │
           Private area        Private area
```

The entire apartment complex is like **AWS**.

Your apartment is like your **VPC**.

Other AWS customers have their own isolated networking environments.

```text
AWS
│
├── Customer A VPC
│
├── Customer B VPC
│
├── Customer C VPC
│
└── Customer D VPC
```

These VPCs are logically isolated from each other.

---

# 4. VPC Architecture — High Level

![Image](https://images.openai.com/static-rsc-4/X3YbL4avGrjnJ2pUart7Z9dnXbhO_UBmyMCgDRgj-eywzonGuuuIAyS810UWH4hx5o4SZkkAid71Fw7A8FUuWd85miC6EzeYh02vAqLLGus0oGDZRY05LVgtTbU11XX-OnxSE0zPIQ3XtIU6XwPyb27wmMHY-smoiyFyxDdxhOKKM5XL8vZfTZD9DwuNJdqR?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/Pscy8YDFHaIDHAK9ySVFbbNakNYRBHgJ69FmzSQp_hals7SaudrQxHs1bxMSCPUVroTN8isiGqlr-J7VoG1pFaEZl-MuJjHkYiARTVwwrVii9P3T6R7ks5YHnwOPLupSDc1NwOBvp3qr805QBi2Nh4nk66GuYXt9FCVQwRJ3hpgQHk5QIR3vuolYEQJyv25T?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/jE1hrPyTaislYBebPiMpJdyNco_znK0g9cn2c13QCquetPANym76SGmKvDG4QIgEhweypoEi0JPAC3SB5p0SbUAl4uRGCG6FduwoGeja98V50xxgb2htQrmIT4EEF9KUsP9PND2ucPSg7btD-Z6xtkcgMDSJwzdeMgtWuCSd4uUfq-tuiGYZl-m3lRRfk9DW?purpose=fullsize)

At a high level:

```text
                         AWS CLOUD
                             │
              ┌──────────────┴──────────────┐
              │                             │
              │          YOUR VPC            │
              │                             │
              │      10.0.0.0/16            │
              │                             │
              │   ┌─────────────────────┐   │
              │   │ AWS Resources       │   │
              │   │                     │   │
              │   │ EC2                 │   │
              │   │ RDS                 │   │
              │   │ Load Balancers      │   │
              │   │ EKS                 │   │
              │   └─────────────────────┘   │
              │                             │
              └─────────────────────────────┘
```

The key point is that **VPC defines the overall virtual network environment**.

---

# 5. VPC is a Regional Resource

An important concept:

> **An AWS VPC belongs to one AWS Region.**

For example:

```text
AWS
│
├── Mumbai Region
│      └── VPC
│
├── Singapore Region
│      └── VPC
│
└── US East Region
       └── VPC
```

A VPC cannot span multiple AWS Regions.

If your application operates in multiple Regions, you generally create separate VPCs in those Regions and connect them using appropriate AWS networking services.

---

# 6. VPC and Availability Zones

A VPC is associated with a Region and can contain resources distributed across multiple **Availability Zones (AZs)** in that Region.

For example:

```text
                    AWS Region
                        │
              ┌─────────┴─────────┐
              │       VPC         │
              │    10.0.0.0/16    │
              │                   │
        ┌─────┴─────┐       ┌─────┴─────┐
        │    AZ-A   │       │    AZ-B   │
        │            │       │           │
        │ Resources  │       │ Resources │
        └────────────┘       └───────────┘
```

This allows applications to use multiple Availability Zones for resilience.

**Important:** AZs are not the same thing as VPCs. A VPC is regional; Availability Zones are separate infrastructure locations within that Region.

---

# 7. VPC CIDR Block

When creating a VPC, you define an IPv4 CIDR block.

Example:

```text
10.0.0.0/16
```

This represents the address range available within the VPC's IPv4 network.

```text
VPC
10.0.0.0/16
```

The `/16` means:

```text
Network bits = 16
Host bits    = 16
```

So the address space contains:

```text
2¹⁶ = 65,536
```

IPv4 addresses in the CIDR block.

**Note:** AWS reserves some IPv4 addresses when you later create subnets, but subnet behavior is outside this lesson.

---

# 8. Common VPC CIDR Examples

You may see:

```text
10.0.0.0/16
10.10.0.0/16
172.16.0.0/16
192.168.0.0/16
```

For a training environment, a common design is:

```text
VPC
10.0.0.0/16
```

Then the network can be divided into smaller ranges later.

For now, remember:

> **VPC CIDR defines the IPv4 address space of the VPC.**

---

# 9. VPC IPv4 Addressing

A VPC can have IPv4 addressing.

For example:

```text
VPC
10.0.0.0/16
```

Conceptually:

```text
10.0.0.0
      │
      ├── 10.0.x.x
      ├── 10.1.x.x
      └── ...
```

The exact addresses assigned to resources depend on the networking configuration.

---

# 10. Default VPC

AWS accounts historically have a **default VPC** available in each Region when applicable.

The default VPC is designed to make it easy to launch resources without first designing a custom network.

Conceptually:

```text
AWS Account
      │
      ├── Default VPC
      │
      └── Custom VPCs
```

A default VPC comes with AWS-managed/default networking configuration that makes common resource launches easier.

For production environments, organizations often create their own VPC designs according to their networking, security, and operational requirements.

---

# 11. Custom VPC

Instead of using the default VPC, you can create your own VPC.

Example:

```text
VPC Name:
Production-VPC

CIDR:
10.0.0.0/16
```

Another example:

```text
VPC Name:
Development-VPC

CIDR:
10.10.0.0/16
```

This gives you separate network environments.

```text
AWS Account
│
├── Production-VPC
│      └── 10.0.0.0/16
│
└── Development-VPC
       └── 10.10.0.0/16
```

---

# 12. Multiple VPCs

An AWS account can have multiple VPCs, subject to AWS quotas.

For example:

```text
                 AWS Account
                     │
        ┌────────────┼────────────┐
        ↓            ↓            ↓
   Production     Staging     Development
      VPC           VPC           VPC
   10.0.0.0/16   10.1.0.0/16   10.2.0.0/16
```

This is useful for separating environments.

For example:

* Production
* Development
* Testing
* Staging

---

# 13. VPC Isolation

VPCs provide logical isolation.

For example:

```text
VPC-A                         VPC-B
10.0.0.0/16                  10.1.0.0/16
   │                             │
   │                             │
Resources                    Resources
```

By default, resources in separate VPCs cannot simply communicate with each other just because they are both inside AWS.

Connectivity between VPCs requires an appropriate networking mechanism and configuration.

---

# 14. VPC Does Not Mean "Internet"

This is a very important point for students.

A VPC is **not automatically the Internet**.

Think:

```text
VPC
│
└── Private virtual network
```

Internet connectivity is a separate networking requirement.

A VPC can be designed for:

```text
Internet-facing applications
```

or:

```text
Private applications
```

or a combination.

So:

> **VPC = Network boundary**
> **Internet = External network**

Do not treat them as the same thing.

---

# 15. VPC and AWS Resources

Many AWS services use VPC networking.

For example:

```text
                    VPC
                     │
       ┌─────────────┼──────────────┐
       ↓             ↓              ↓
      EC2           RDS            EKS
       │             │              │
       └─────────────┼──────────────┘
                     ↓
                Application
```

Examples of resources/services that can use VPC networking include:

* EC2
* RDS
* EKS
* Elastic Load Balancing
* ElastiCache
* ECS
* Lambda functions configured for VPC access

The exact networking behavior differs by service.

---

# 16. VPC and EC2

A very common DevOps example:

```text
                     AWS Region
                         │
                         ↓
                       VPC
                    10.0.0.0/16
                         │
                         ↓
                        EC2
```

When launching an EC2 instance, you choose its VPC/networking placement.

This means the EC2 instance gets network connectivity within the VPC according to the associated networking configuration.

---

# 17. VPC and RDS

A database can also be associated with VPC networking.

Example:

```text
              VPC
               │
       ┌───────┴────────┐
       │                │
     EC2               RDS
     App              Database
```

The application and database can communicate over the VPC network when the relevant network and security configuration allows it.

---

# 18. VPC and EKS

This is particularly important for DevOps.

Amazon EKS clusters use AWS networking and can integrate deeply with VPC networking.

Simplified view:

```text
                 VPC
                  │
          ┌───────┴────────┐
          │                │
        EKS              Other
       Cluster          Resources
          │
     ┌────┼────┐
     ↓    ↓    ↓
    Pod  Pod  Pod
```

This is why understanding VPC is essential before learning **EKS networking**.

---

# 19. VPC and Multi-Tier Applications

A typical application architecture can be represented at a high level as:

```text
                         VPC
                          │
             ┌────────────┼────────────┐
             │            │            │
             ↓            ↓            ↓
          Frontend     Backend       Database
             │            │            │
             └────────────┼────────────┘
                          │
                     Application
```

Later, these application tiers can be placed into different network segments and controlled using AWS networking and security features.

We are intentionally **not covering those components here**.

---

# 20. VPC Peering — Basic Concept

Sometimes two VPCs need to communicate.

Example:

```text
VPC-A                       VPC-B
10.0.0.0/16                 10.1.0.0/16
   │                            │
   └──────── VPC Peering ───────┘
```

**VPC peering** provides private connectivity between two VPCs.

Important:

> VPC peering is **not required simply because two VPCs exist**.

It is used when communication between them is required and the network design calls for it.

---

# 21. VPC and AWS Transit Gateway

For a small number of VPCs, direct connectivity may be manageable.

But imagine:

```text
VPC-A
VPC-B
VPC-C
VPC-D
VPC-E
VPC-F
```

Connecting every VPC directly can become difficult to manage.

AWS **Transit Gateway** can provide a central connectivity architecture.

```text
                 Transit Gateway
                /   /   |   \   \
               /   /    |    \   \
             VPC VPC   VPC   VPC  VPC
```

This is commonly used in larger AWS environments.

---

# 22. VPC and Hybrid Cloud

VPC can also be part of a hybrid architecture.

For example:

```text
          On-Premises Data Center
                    │
                    │
             VPN / Direct Connect
                    │
                    ↓
                  AWS
                    │
                   VPC
                    │
             AWS Applications
```

This allows an organization to connect its on-premises network with AWS networking.

---

# 23. VPC DNS

VPC also has DNS-related functionality.

When you create a VPC, AWS provides DNS support features that allow resources in the VPC to use DNS resolution.

Conceptually:

```text
EC2
 │
 │ DNS Query
 ↓
AWS DNS Resolver
 │
 ↓
IP Address
```

This is important because applications normally communicate using hostnames as well as IP addresses.

For example:

```text
database.internal
       ↓
10.x.x.x
```

---

# 24. VPC Tenancy

When creating a VPC, AWS provides a tenancy setting.

The traditional options include:

```text
default
dedicated
```

**Default tenancy** allows eligible EC2 instances to run on shared AWS hardware infrastructure.

**Dedicated tenancy** is used for certain requirements where instances run on hardware dedicated to a single AWS account.

For most standard learning and application scenarios, you will commonly encounter **default tenancy**.

---

# 25. IPv6 in VPC

AWS VPC also supports **IPv6**.

So a VPC can use:

```text
IPv4
+
IPv6
```

Example concept:

```text
VPC
│
├── IPv4 CIDR
│      10.0.0.0/16
│
└── IPv6 CIDR
       2001:db8:....
```

IPv6 uses **128-bit addresses**, compared with IPv4's 32-bit addresses.

For your course, teach IPv4 first, then introduce IPv6 after students understand CIDR and subnetting.

---

# 26. VPC — Complete High-Level Architecture

Here is the architecture students should remember:

```text
                              AWS CLOUD
                                  │
                    ┌─────────────┴─────────────┐
                    │                           │
                    │          REGION           │
                    │                           │
                    │     ┌───────────────┐     │
                    │     │      VPC      │     │
                    │     │               │     │
                    │     │ 10.0.0.0/16   │     │
                    │     │               │     │
                    │     │ ┌───────────┐ │     │
                    │     │ │ Resources │ │     │
                    │     │ │           │ │     │
                    │     │ │ EC2       │ │     │
                    │     │ │ RDS       │ │     │
                    │     │ │ EKS       │ │     │
                    │     │ └───────────┘ │     │
                    │     │               │     │
                    │     └───────────────┘     │
                    │                           │
                    └───────────────────────────┘
```

The **VPC is the overall virtual network boundary**.

---

# 27. VPC vs Traditional Data Center Network

Students coming from traditional networking can understand it like this:

| Traditional Data Center | AWS                               |
| ----------------------- | --------------------------------- |
| Physical network        | VPC                               |
| Network address space   | VPC CIDR                          |
| Physical servers        | EC2 / other compute               |
| Physical infrastructure | AWS infrastructure                |
| Data center network     | VPC network                       |
| Physical network design | Software-defined cloud networking |

The key difference is that AWS provides networking as **software-defined infrastructure**.

You can create and modify networking resources through:

* AWS Management Console
* AWS CLI
* SDKs
* CloudFormation
* Terraform

---

# 28. VPC Through Terraform

For DevOps students, this is important.

A VPC can be created using Terraform.

Conceptually:

```text
Terraform
    │
    ↓
AWS Provider
    │
    ↓
Create VPC
    │
    ↓
10.0.0.0/16
```

A simplified Terraform resource looks like:

```hcl
resource "aws_vpc" "main" {
  cidr_block = "10.0.0.0/16"

  tags = {
    Name = "devops-vpc"
  }
}
```

This creates the **VPC itself**.

It does **not** by itself teach or configure the other networking components we're intentionally excluding from this lesson.

---

# 29. VPC — Interview Definition

### Short interview answer

> **AWS VPC stands for Virtual Private Cloud. It is a logically isolated virtual network inside an AWS Region where we can run AWS resources with our own IP address space and networking configuration. We define the VPC's IP range using CIDR, such as 10.0.0.0/16. A VPC provides the network boundary for cloud applications and can support both IPv4 and IPv6 networking.**

---

# 30. What Students Should Remember

```text
                    AWS VPC
                       │
          ┌────────────┼────────────┐
          ↓            ↓            ↓
      Virtual       Network       AWS
      Network       Boundary      Resources
          │
          ↓
       CIDR Block
          │
          ↓
     Regional Scope
          │
          ↓
    Logical Isolation
```

### Five key points

**1. VPC = Virtual Private Cloud**

**2. VPC is a logical network inside AWS**

**3. VPC is Regional**

**4. VPC has an IP address range defined using CIDR**

**5. VPC provides the network boundary for AWS resources**

---

## What comes after VPC?

For your **Module 3 — Networking**, the teaching flow can be:

```text
IPv4
  ↓
AWS VPC                     ← TODAY
  ↓
VPC Subnet
  ↓
Route Tables
  ↓
Internet Gateway
  ↓
NAT Gateway
  ↓
Security Groups
  ↓
NACL
  ↓
VPC Peering
  ↓
Transit Gateway
  ↓
Load Balancer
  ↓
DNS
  ↓
Complete AWS VPC Architecture
```
