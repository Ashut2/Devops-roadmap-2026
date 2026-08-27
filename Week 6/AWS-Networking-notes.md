# AWS Networking — Day 9a Notes

## 1. CIDR & Subnetting
##  What does /16 vs /24 actually mean ?

-> The difference comes down to network size versus host capacity. 
A 16-bit subnet mask fixes the first 16 bits to the network and the remaining 16 for the host,
giving you around 65,000 IP addresses. In cloud design we typically assign a 16-bit subnet mask
to the entire VPC, like 10.0.0.0/16.  /24-bit subnet mask fixes the first 24 bits, leaving only
eight bits for the host, giving you 256 total IPs (or 251 usable in AWS because five bits are kept by AWS itself).
This is the standard size we cover for individual subnets such as our public, private, or data 
-base tiers. In short a 16-bit subnet mask is a large address space for the whole network and
24-bit subnet masks are smaller slices we use to isolate our tiers 

`Example Breakdown`
`10.0.0.0/16`:

Network Portion: 10.0. is fixed.

Host Range: 10.0.0.0 to 10.0.255.255.

Can be carved up into 256 smaller /24 subnets (10.0.0.0/24, 10.0.1.0/24, ..., 10.0.255.0/24).

`10.0.1.0/24`:

Network Portion: 10.0.1. is fixed.

Host Range: 10.0.1.0 to 10.0.1.255.

Ideal for a single tier (like the public frontend, backend, or isolated DB subnet).

---
## Show math: how many usable IPs in a /24, and why is it not the full 256?

->  /24 have 24 bit fixed for network & remaining 8 bit for host 
- So, Total no. of Usable Ip address = 2^(host-bits) - 2 => 256
- but AWS reserved first 4 and last IP for different components & purposes,
so total usable Ip = 256 - 5 = 251 IPs 


## Why does AWS reserve 5 IPs per subnet — what are they for?

->

| Reserved IP | AWS Designation | Purpose |
|---|---|---|
| **`10.0.1.0`** (Base / 1st IP) | **Network Address** | The formal network identifier (standard IP networking rule). |
| **`10.0.1.1`** (Base + 1) | **VPC Router** | The default gateway / virtual router for the subnet that handles routing between subnets and out to gateways (IGW/NAT). |
| **`10.0.1.2`** (Base + 2) | **Amazon DNS Server (Route 53 Resolver)** | Handles DNS resolution within the VPC (resolving domain names to IP addresses). |
| **`10.0.1.3`** (Base + 3) | **Future Use** | Reserved by AWS for future internal networking features. |
| **`10.0.1.255`** (Last IP) | **Broadcast Address** | Standard network broadcast address. AWS VPCs do not support broadcast traffic, so this address is reserved/disabled. |

## 2. Why a VPC needs subnets at all
`What problem does splitting a VPC into subnets actually solve?`

- splitting of subnets introduces security, scalability and reliability in our Infrastructure. 
- It solves the problem of getting data compromised easily because of the fact that db is internet facing due to deployment of whole web-app over a flat network. 


` why not just put everything in one big flat network? `

- Because doing so would increase the blast radius & increase the surface area making web application vulnerable/prone to attackers 

---
## 3. Public vs Private Subnet
` What technically makes a subnet "public" — is it a property of the subnet itself, or something else?`

- Its because of the IGW (internet gateway) which we attach to the subnet makes it public.
- It allows both inbound & outbound traffic towards this subnet from internet via IGW & NAT
gateway

`what has to be true for an instance in a subnet to be reachable from the internet?`


## 4. Three-Tier Architecture
- Sketch (in words or ASCII) the Web → App → DB tier pattern.
- Why does the DB tier not need a route to the internet at all?
- What's the actual security benefit of this separation — what does it prevent?

## 5. Multi-AZ
- Why is "1 subnet = 1 AZ" a hard rule, not a convention?
- What real failure does spreading instances across AZ-a and AZ-b protect against?

## 6. Security Groups vs NACLs
- Fill this comparison in your own words, don't just restate the stateful/stateless label:

| | Security Group | NACL |
|---|---|---|
| Applies at | | |
| Stateful or stateless? | | |
| What "stateless" actually means for return traffic | | |
| Default behavior (allow/deny) | | |
| When would you actually reach for a NACL if SGs already exist? | | |

## 7. What I'd still get wrong in an interview
- Honest section — what's still shaky if someone pushed back on you right now?

## 8. Open questions
- Anything you're not 100% sure about, to revisit later.
