# AWS Internet Gateway (IGW)

An **Internet Gateway (IGW)** is an AWS networking component that allows resources inside an **AWS VPC** to communicate with the **public Internet**.

In simple words:

> **Internet Gateway is the door between your VPC and the Internet.**

It is commonly used when you want a public-facing resource such as an **EC2 instance** to access the Internet or be accessed from the Internet.

---

## 1. Basic Architecture

Assume we have:

* VPC: `10.0.0.0/16`
* Public Subnet: `10.0.1.0/24`
* EC2 instance: `10.0.1.10`
* Internet Gateway: `IGW`
* Route Table with Internet route

```text
                    INTERNET
                        |
                        |
                +---------------+
                | Internet      |
                | Gateway (IGW) |
                +---------------+
                        |
                        |
              +-------------------+
              |       AWS VPC     |
              |    10.0.0.0/16    |
              |                   |
              |  +-------------+  |
              |  | Public      |  |
              |  | Subnet      |  |
              |  | 10.0.1.0/24 |  |
              |  |             |  |
              |  | EC2         |  |
              |  | 10.0.1.10   |  |
              |  +-------------+  |
              +-------------------+
```

The important point is that **IGW alone does not make a subnet public**.

You need the correct **route table** and an EC2 instance with a **public IPv4 address** or other supported public connectivity.

---

# 2. Why Do We Need an Internet Gateway?

An AWS VPC is logically isolated from the Internet.

For example:

```text
VPC
10.0.0.0/16
       |
       |
     EC2
10.0.1.10
       |
       X
    Internet
```

The EC2 instance cannot directly communicate with the Internet just because it exists inside a VPC.

We need:

```text
EC2
 |
 | Route
 v
Internet Gateway
 |
 v
Internet
```

---

# 3. How Internet Gateway Works

Consider an EC2 instance:

```text
Private IP = 10.0.1.10
Public IP  = 13.x.x.x
```

The EC2 wants to access:

```text
Google / Internet
8.8.8.8
```

The traffic flow is:

```text
EC2
10.0.1.10
   |
   | Destination: 8.8.8.8
   v
Route Table
   |
   | 0.0.0.0/0 → IGW
   v
Internet Gateway
   |
   v
Internet
   |
   v
8.8.8.8
```

The return traffic comes back through the Internet Gateway to the EC2.

---

# 4. Internet Gateway and Route Table

This is one of the most important concepts.

Suppose your VPC is:

```text
VPC
10.0.0.0/16
```

And your public subnet is:

```text
10.0.1.0/24
```

Your route table might contain:

| Destination   | Target           |
| ------------- | ---------------- |
| `10.0.0.0/16` | local            |
| `0.0.0.0/0`   | Internet Gateway |

The important route is:

```text
0.0.0.0/0 → igw-xxxxxxxx
```

This means:

> "For any destination that is not inside my VPC, send the traffic to the Internet Gateway."

---

# 5. How to Create an Internet Gateway

Go to:

**AWS Console → VPC → Internet Gateways**

### Step 1 — Create IGW

Click:

**Create internet gateway**

Give it a name:

```text
My-IGW
```

Click:

**Create internet gateway**

You will get something like:

```text
igw-0123456789abcdef
```

---

# 6. Attach IGW to VPC

Creating an Internet Gateway is not enough.

You must attach it to your VPC.

```text
Internet Gateway
       |
       |
       v
      VPC
10.0.0.0/16
```

From the Internet Gateway page:

**Actions → Attach to a VPC**

Select:

```text
My-VPC
10.0.0.0/16
```

Then attach it.

---

# 7. Configure the Route Table

Now go to:

**VPC → Route Tables**

Select the route table associated with your public subnet.

Add:

```text
Destination: 0.0.0.0/0

Target: Internet Gateway
        igw-xxxxxxxx
```

Your route table becomes:

```text
Destination       Target
--------------------------------
10.0.0.0/16       local
0.0.0.0/0         igw-xxxxxxxx
```

---

# 8. Public Subnet

A subnet becomes a **public subnet** when its associated route table has a route to an Internet Gateway.

For example:

```text
VPC
10.0.0.0/16
        |
        +----------------------+
        |                      |
        v                      v
Public Subnet             Private Subnet
10.0.1.0/24               10.0.2.0/24
        |                      |
        v                      v
      EC2                    EC2
        |
        |
        v
       IGW
        |
        v
    INTERNET
```

Public subnet route table:

```text
0.0.0.0/0 → IGW
```

Private subnet route table:

```text
No direct route to IGW
```

---

# 9. Does IGW Give EC2 a Public IP?

**No.**

This is a very common interview question.

An Internet Gateway **does not automatically give an EC2 instance a public IP address**.

For an EC2 instance to communicate directly with the Internet through an IGW, it generally needs:

1. A subnet whose route table has a route to the IGW
2. A public IPv4 address or Elastic IP
3. Appropriate Security Group rules
4. Appropriate Network ACL rules

Example:

```text
EC2
Private IP: 10.0.1.10
Public IP:  54.x.x.x
      |
      v
Route Table
0.0.0.0/0 → IGW
      |
      v
Internet Gateway
      |
      v
Internet
```

---

# 10. IGW vs NAT Gateway

This is another important interview topic.

### Internet Gateway

Used for **public Internet connectivity**.

```text
Public EC2
    |
    v
   IGW
    |
    v
Internet
```

### NAT Gateway

Used to allow **private subnet resources to initiate outbound Internet connections** without making those resources directly reachable from the Internet.

```text
Private EC2
    |
    v
NAT Gateway
    |
    v
Internet Gateway
    |
    v
Internet
```

Example:

```text
                 INTERNET
                     |
                     |
                    IGW
                     |
              +------+------+
              |             |
              v             v
        Public Subnet   NAT Gateway
              |             |
            EC2        Private Subnet
                            |
                           EC2
```

---

# 11. Important Characteristics of Internet Gateway

| Feature                          | Internet Gateway |
| -------------------------------- | ---------------- |
| Used with                        | VPC              |
| Provides Internet connectivity   | Yes              |
| Horizontally scalable            | Yes              |
| Managed by AWS                   | Yes              |
| Requires provisioning capacity   | No               |
| Supports IPv4                    | Yes              |
| Supports IPv6                    | Yes              |
| Attached to                      | VPC              |
| Can be attached to multiple VPCs | No               |
| Provides public IP automatically | No               |
| Used by public subnet            | Yes              |

---

# 12. Internet Gateway vs Router

Think of it this way:

```text
VPC
 |
 | Route Table
 |
 | 0.0.0.0/0
 |
 v
Internet Gateway
 |
 v
Internet
```

The **Route Table decides where traffic should go**.

The **Internet Gateway provides the connection between the VPC and Internet**.

So:

> **Route Table = Traffic direction**
> **Internet Gateway = Internet connectivity**

---

# 13. Real-Time Example

Suppose you are deploying an e-commerce application.

```text
                 INTERNET
                     |
                     v
             Internet Gateway
                     |
                     v
              Public Subnet
              10.0.1.0/24
                     |
             +-------+-------+
             |               |
             v               v
          Web EC2         Load Balancer
```

Your VPC:

```text
10.0.0.0/16
```

Public subnet:

```text
10.0.1.0/24
```

Route table:

```text
10.0.0.0/16 → local
0.0.0.0/0   → IGW
```

Now users on the Internet can access the public application, subject to the load balancer/EC2 security controls.

---

# 14. Important Interview Question

### Q: Can we attach an Internet Gateway directly to a subnet?

**No.**

The relationship is:

```text
Internet Gateway
       |
       v
      VPC
       |
       v
  Route Table
       |
       v
    Subnet
```

The IGW is attached to the **VPC**, not directly to the subnet.

---

## 15. Complete AWS VPC Internet Flow

Remember this architecture:

```text
                         INTERNET
                            |
                            |
                     +-------------+
                     |     IGW     |
                     +-------------+
                            |
                            |
                     +-------------+
                     |     VPC     |
                     | 10.0.0.0/16 |
                     +-------------+
                            |
                     Route Table
                     0.0.0.0/0
                         → IGW
                            |
                            v
                    Public Subnet
                    10.0.1.0/24
                            |
                            v
                          EC2
                    Private IP
                    10.0.1.10
                    Public IP
                    54.x.x.x
```

### Easy way to remember

**VPC → Route Table → IGW → Internet**

And for a private server:

**Private Subnet → NAT Gateway → IGW → Internet**
