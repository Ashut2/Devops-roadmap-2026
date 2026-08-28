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

The following things need to be true for an instance in a subnet to be reachable from the internet : 
  - Quick Memory Trigger: "P-I-R-N-S"


**Public IP (Address)** : All the instance have default private Ip, we need to enable `automatically assign IPv4 elastic Ip to this instance, it get assigned to its Elastic network interface (ENI)` option.


**IGW (VPC Gate)**: The Internet gateway has to be attached to the VPC. It fascilitates the translation of public Ip to internal private Ip & vice versa according to the case of whether request is being sent out to the internet or coming from the internet. 

**Route Table (Direction)**: It acts as a sign board so it contains a default route path to IGW 

    Destination: 0.0.0.0/0 -> Target:igw-xxxxxxxx 

**NACL (Subnet Barrier)**: Its a subnet level stateless firewall. 

- It doesn't keep track of allowed Ips , so we have to explicitly set both inbound and outbound rules for every single Ip.
- allows deny rules. 
- must allow:

    - Inbound: Traffic on the destination port.

    - Outbound: Return traffic on ephemeral ports (1024–65535), because NACLs are stateless and do not automatically track connections.


**Security Group (Instance Barrier)**: Its an Instance level firewall. These are just security rules we define explicitly to add Allowed ips to access our instance. 

- The instance's attached Security Group must have an inbound rule allowing the required protocol and port (e.g., TCP 80/443/22) from the internet (0.0.0.0/0 or specific public source CIDRs).


## 4. Three-Tier Architecture
### Sketch (in words or ASCII) the Web → App → DB tier pattern.
    web tier 
      - its the frontend tier deployed on a public subnet. 
      - This tier allows both ingress and egress traffic from internet (0.0.0.0/0) with the help of IGW. 
      - amazon load balancer sits here to manage the traffic
      - route table route: Destination:0.0.0.0/0 -> Target:igw-xxxxxxxx

      
    app tier
      - its the logical tier of our web application. 
      
      - contains APIs/pods 
      
      - deployed on a private subnet with 
        -> outbound connectivity rule towards database tier to query data.
        -> Inbound from web tier to receive api requests
        -> outbound to internet (NAT) to fetch container images, packages etc 
      
      - NAT gateway sits on public subnet (web tier)
      
      - app tier routes its outbound traffic towards the NAT gateway sitting in public subnet. 
      
      - route table routes: 0.0.0.0/0 -> nat-xxxxxxxx

    Database tier
      - its the storage tier of our web app.
      - Contains data & information about business and operations & decisions etc
      - RDS Postgress sits here. 
      - deployed on a private subnet.
      - It is no internet route at all.
      - talks only locally to the app tier.  

| Tier | Subnet Type | Components Located Here | Inbound Traffic | Outbound Traffic | Route Table (`0.0.0.0/0`) |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Web Tier** | Public | ALB, **NAT Gateway** | Internet (`0.0.0.0/0`) via IGW | Direct to Internet via IGW & Local App Tier | `Target: igw-xxxxxxxx` |
| **App Tier** | Private | EC2 / EKS Pods / APIs | Only from Web Tier (ALB) | Internet (via NAT GW) & Local DB Tier | `Target: nat-xxxxxxxx` |
| **DB Tier** | Isolated / Private | RDS PostgreSQL | Only from App Tier | Only return traffic to App Tier | *None* (Local VPC route only) |


### Why does the DB tier not need a route to the internet at all?

- The DB Tier does not need an internet route because its only client is the internal App Tier via local VPC routing. Removing both ingress (via IGW) and egress (via NAT GW) enforces the principle of least privilege, reduces the attack surface, and prevents outbound data exfiltration


### What's the actual security benefit of this separation — what does it prevent?

- Direct public ingress : prevents unauthenticated access & port scanning on the application workloads and Databases.

- lateral pivot: confines the attacker to the the compromised web tier. prevents immediate traversal to backend storage

- Data Exfiltration: let's suppose database engine got compromised, so because it is isolated from the internet, stolen data cannot be transmit to other servers.  Database communicates with the other subnets with local vpc route (Target: local)   


## 5. Multi-AZ
- Why is "1 subnet = 1 AZ" a hard rule, not a convention?

      Aws have this hard rule.
      AZ-availability zones are physical data centers AWS have deployed in different regions
      subnets are tied to physical locations in the different data center (az - ap-south-1a or ap-south-1b).
      Hence they cannot span across multiple az's

      High availability can be achieved by creating identical infrastructure across two AZ's
      so if one server goes on fire, the other one gets 100% traffic directed by ALB (load balancer)

      `Gemini polished version`
        AWS enforces 1 Subnet = 1 AZ as a hard physical constraint because a subnet maps to local hardware
        and switching domains within a specific Availability Zone facility. Subnets cannot span AZs.
        To build High Availability (HA), we deploy parallel, identical subnets and application instances across
        two or more AZs. An Application Load Balancer (ALB) sits in front, distributing traffic evenly and
        automatically shifting 100% of traffic to the healthy AZ via target health checks if one AZ experiences
        a physical failure or outage.

      
- What real failure does spreading instances across AZ-a and AZ-b protect against?

      Spreading instances across AZ-a & AZ-b protect against isolated physical data center
      and zonal infra failure such as fiber cuts, cooling failures, localized power outage,
      bad AWS hardware updates et cetera 
      Because Availability zones are physically separated facilities with independent fault
      domains, an entire facility failing in AZ-a would not impact AZ-b, allowing load balancer
      to keep the application online with zero downtime. 
 - What it doesn't protect against

        the boundaries of Multi-AZ:
         1) Region-Wide Outages: If the entire ap-south-1 region loses its control plane
         (like IAM or Route 53 regional endpoints), Multi-AZ will not save you (that requires Multi-Region).
   
         2) Application Bugs / Bad Code: If your code contains a bug that panics on startup,
         deploying it to both AZs will simply crash instances in both AZs simultaneously.
   
         3) Corrupted Database Writes: Multi-AZ replication syncs data changes; a bad SQL query
            or dropped table replicates instantly across both zones.

## 6. Security Groups vs NACLs

| | Security Group | NACL |
|---|---|---|
| Applies at |instance level | subnet level |
| Stateful or stateless? |stateful | stateless |
| What "stateless" actually means for return traffic |N/A (Stateful): Return traffic is automatically tracked and allowed, regardless of outbound rules |Every packet is evaluated independently. If an inbound request is permitted, response packets will be dropped unless an explicit outbound rule allows traffic back out on temporary ephemeral ports (1024–65535) |
| Default behavior (allow/deny) |Deny all inbound by default. Supports Allow-only rules (everything not explicitly allowed is blocked) |`Default NACL:` Allows all inbound and outbound (* / 0.0.0.0/0). `Custom NACL:` Denies all inbound and outbound until numbered Allow/Deny rules are defined. |
| When would you actually reach for a NACL if SGs already exist? |Default choice for micro-segmenting applications and defining instance-level access|`1. Explicit IP/Subnet Blacklisting`: Blocking specific malicious IPs/ranges (SGs cannot do Deny rules).  `2. Broad Perimeter Defense`: Enforcing a baseline security policy or compliance rule across all resources in a subnet at once. `3. DDoS/Abuse Mitigation`: Dropping unwanted traffic at the subnet edge before it hits instances or burns compute/ENI limits. |

`summarized last point`  ->  Use Security Groups for day-to-day access (e.g., "Allow App server to talk to DB"). 
Reach for a NACL when you need to explicitly block a bad IP or enforce an unbreakable baseline rule across an 
entire subnet.

## 7. What I'd still get wrong in an interview
- Honest section — what's still shaky if someone pushed back on you right now?

- 3 tier architecture
- how instance reaches internet
- Why vertical isolation & horizontal HA ?

## 8. Open questions
- Anything we're not 100% sure about, to revisit later.
