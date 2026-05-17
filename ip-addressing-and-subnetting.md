# IP Addressing and Subnetting

Notes from Networking module — Pearson BTEC Level 5 HND
in Computing (Software Engineering)
ISMT College Kathmandu — Amir Maharjan

---

## 1. IP Addressing Overview

An IP address is a unique numerical label assigned to every
device connected to a network. It serves two main purposes:

- **Host identification** — identifies a specific device
- **Location addressing** — identifies where the device is
  on the network

There are two versions currently in use:
- **IPv4** — 32-bit address — current standard
- **IPv6** — 128-bit address — gradually replacing IPv4

---

## 2. IPv4 Addressing

### Structure
- 32 bits long
- Written in dotted decimal notation
- Divided into four octets of 8 bits each
- Each octet ranges from 0 to 255

**Example:** 192.168.1.100

| Octet 1 | Octet 2 | Octet 3 | Octet 4 |
|---------|---------|---------|---------|
| 192 | 168 | 1 | 100 |
| 11000000 | 10101000 | 00000001 | 01100100 |

### Network and Host Portions
Every IPv4 address has two parts:
- **Network portion** — identifies the network
- **Host portion** — identifies the specific device

The subnet mask determines where the network portion ends
and the host portion begins.

---

## 3. Subnet Masks

A subnet mask is a 32-bit number that masks an IP address
and divides it into network and host portions.

### Common Subnet Masks

| CIDR | Subnet Mask | Network Bits | Host Bits | Usable Hosts |
|------|------------|--------------|-----------|--------------|
| /8 | 255.0.0.0 | 8 | 24 | 16,777,214 |
| /16 | 255.255.0.0 | 16 | 16 | 65,534 |
| /24 | 255.255.255.0 | 24 | 8 | 254 |
| /25 | 255.255.255.128 | 25 | 7 | 126 |
| /26 | 255.255.255.192 | 26 | 6 | 62 |
| /27 | 255.255.255.224 | 27 | 5 | 30 |
| /28 | 255.255.255.240 | 28 | 4 | 14 |
| /29 | 255.255.255.248 | 29 | 3 | 6 |
| /30 | 255.255.255.252 | 30 | 2 | 2 |

**Usable hosts = 2^(host bits) - 2**
Subtract 2 for network address and broadcast address

---

## 4. Special IP Address Ranges

### Private IP Ranges
Not routable on the public internet — used for internal
networks only:

| Range | CIDR | Common Use |
|-------|------|------------|
| 10.0.0.0 – 10.255.255.255 | 10.0.0.0/8 | Large enterprises |
| 172.16.0.0 – 172.31.255.255 | 172.16.0.0/12 | Medium networks |
| 192.168.0.0 – 192.168.255.255 | 192.168.0.0/16 | Home and small office |

### Other Special Ranges

| Range | Purpose |
|-------|---------|
| 127.0.0.0/8 | Loopback — 127.0.0.1 refers to local device |
| 169.254.0.0/16 | APIPA — automatic private IP when DHCP fails |
| 0.0.0.0 | Unspecified address |
| 255.255.255.255 | Limited broadcast |

---

## 5. Subnetting

Subnetting is the process of dividing a large network into
smaller subnetworks (subnets).

### Why Subnet?

**Performance:**
- Reduces broadcast traffic within each subnet
- Smaller broadcast domains mean less unnecessary traffic
- Better network performance overall

**Security:**
- Isolates different parts of network from each other
- Firewall rules applied between subnets
- Compromised device cannot easily reach other subnets
- Critical systems placed on isolated subnets

**Management:**
- Easier to identify and organise devices
- Simpler troubleshooting
- Better IP address management

---

## 6. Subnetting Examples

### Example 1 — Dividing /24 into 4 Subnets

**Given network:** 192.168.1.0/24
**Requirement:** 4 subnets

**Step 1 — Calculate bits needed:**
2^n >= 4 — need n = 2 bits
New prefix = /24 + 2 = /26

**Step 2 — Calculate subnet details:**
- Subnet mask: 255.255.255.192
- Hosts per subnet: 2^6 - 2 = 62 usable hosts
- Number of subnets: 4

**Step 3 — List subnets:**

| Subnet | Network Address | Usable Range | Broadcast |
|--------|----------------|--------------|-----------|
| Subnet 1 | 192.168.1.0/26 | 192.168.1.1 – 192.168.1.62 | 192.168.1.63 |
| Subnet 2 | 192.168.1.64/26 | 192.168.1.65 – 192.168.1.126 | 192.168.1.127 |
| Subnet 3 | 192.168.1.128/26 | 192.168.1.129 – 192.168.1.190 | 192.168.1.191 |
| Subnet 4 | 192.168.1.192/26 | 192.168.1.193 – 192.168.1.254 | 192.168.1.255 |

---

### Example 2 — Dividing /24 into 8 Subnets

**Given network:** 10.0.0.0/24
**Requirement:** 8 subnets

**Step 1 — Calculate bits needed:**
2^n >= 8 — need n = 3 bits
New prefix = /24 + 3 = /27

**Step 2 — Calculate subnet details:**
- Subnet mask: 255.255.255.224
- Hosts per subnet: 2^5 - 2 = 30 usable hosts
- Number of subnets: 8

**Step 3 — List subnets:**

| Subnet | Network Address | Usable Range | Broadcast |
|--------|----------------|--------------|-----------|
| Subnet 1 | 10.0.0.0/27 | 10.0.0.1 – 10.0.0.30 | 10.0.0.31 |
| Subnet 2 | 10.0.0.32/27 | 10.0.0.33 – 10.0.0.62 | 10.0.0.63 |
| Subnet 3 | 10.0.0.64/27 | 10.0.0.65 – 10.0.0.94 | 10.0.0.95 |
| Subnet 4 | 10.0.0.96/27 | 10.0.0.97 – 10.0.0.126 | 10.0.0.127 |
| Subnet 5 | 10.0.0.128/27 | 10.0.0.129 – 10.0.0.158 | 10.0.0.159 |
| Subnet 6 | 10.0.0.160/27 | 10.0.0.161 – 10.0.0.190 | 10.0.0.191 |
| Subnet 7 | 10.0.0.192/27 | 10.0.0.193 – 10.0.0.222 | 10.0.0.223 |
| Subnet 8 | 10.0.0.224/27 | 10.0.0.225 – 10.0.0.254 | 10.0.0.255 |

---

### Example 3 — Real World Small Business Scenario

**Scenario:** Angels Inc wants to separate network into:
- Management network — 2 devices (owners only)
- Point of Sale network — payment processing devices
- Guest WiFi network — customer devices

**Given network:** 192.168.0.0/24

**Solution — use /30 for small subnets:**

| Network | Purpose | Subnet | Usable Hosts |
|---------|---------|--------|--------------|
| Management | Owner devices only | 192.168.0.0/30 | 192.168.0.1 – 192.168.0.2 |
| Point of Sale | Payment devices | 192.168.0.4/30 | 192.168.0.5 – 192.168.0.6 |
| Guest WiFi | Customer devices | 192.168.0.64/26 | 192.168.0.65 – 192.168.0.126 |

**Security benefit:**
- Payment processing devices isolated on their own subnet
- Guest customer devices completely separated from business
  devices and management network
- Firewall rules prevent guest network from accessing
  management or payment subnets

*This real-world application directly relates to my cybersecurity
assessment of Angels Inc — proper network segmentation is one
of the key recommendations for improving the business security
posture.*

---

## 7. IPv6 Addressing

### Why IPv6?
IPv4 provides approximately 4.3 billion addresses. With the
explosion of internet-connected devices — smartphones, IoT
devices, smart appliances — IPv4 addresses are exhausted.
IPv6 provides effectively unlimited addresses.

### IPv6 Structure
- 128 bits long
- Written in hexadecimal notation
- Divided into 8 groups of 16 bits separated by colons
- Each group written as 4 hexadecimal digits

**Example:** 2001:0db8:85a3:0000:0000:8a2e:0370:7334

### IPv6 Simplification Rules
- Leading zeros in each group can be omitted
- One consecutive group of all zeros can be replaced with ::

**Full:** 2001:0db8:0000:0000:0000:0000:0000:0001
**Simplified:** 2001:db8::1

### IPv6 Address Types

| Type | Description | Example |
|------|-------------|---------|
| Unicast | Single interface | 2001:db8::1 |
| Multicast | Multiple interfaces | ff02::1 |
| Anycast | Nearest of multiple interfaces | Same as unicast format |
| Loopback | Local device | ::1 |
| Link-local | Local network only | fe80::/10 |

### IPv6 Security Considerations
- IPSec built into IPv6 — mandatory support
- No NAT — every device has public address — direct exposure
- New attack vectors — neighbour discovery protocol attacks
- Transition mechanisms (6to4, Teredo) introduce vulnerabilities
- Many organisations not yet monitoring IPv6 traffic — blind spot

---

## 8. NAT — Network Address Translation

NAT allows multiple devices on a private network to share
a single public IP address for internet access.

### How NAT Works
1. Device on private network sends packet to internet
2. Router replaces private source IP with public IP
3. Router records mapping in NAT table
4. Response returns to router public IP
5. Router looks up NAT table and forwards to correct
   private device

### Types of NAT

| Type | Description |
|------|-------------|
| Static NAT | One-to-one mapping — private IP always maps to same public IP |
| Dynamic NAT | Pool of public IPs assigned dynamically |
| PAT (Port Address Translation) | Many-to-one — uses port numbers to track connections |

**PAT is most common** — used in home routers and small offices.
Multiple devices share single public IP distinguished by
different port numbers.

### NAT Security Implications
- Hides internal network structure from outside
- Adds layer of obscurity — attacker cannot directly reach
  internal devices
- However NAT is not a security control — it is address translation
- Should not be relied upon as security measure
- Outbound connections can still carry malware
- Does not protect against application layer attacks

---

## 9. Practical Application

### Subnetting in Professional Environment
During my IT Systems Administrator internship at Addon
Engineering Solutions I encountered subnetting concepts
in practical contexts:

- Understanding network topology required reading IP addressing
  scheme of office network
- Troubleshooting required identifying which subnet devices
  belonged to
- Access control configuration required understanding of
  network and host addresses
- Documentation of network configuration included subnet
  information for all network segments

### Subnetting for Security — Key Principle
The most important security application of subnetting is
**network segmentation**:

- Separate sensitive systems on dedicated subnets
- Apply strict firewall rules between subnets
- Limit lateral movement if attacker gains access
- Monitor traffic between subnets for anomalies

**Practical example for Nepal small businesses:**
A small business like Angels Inc should separate:
- Business devices — one subnet
- Payment processing — isolated subnet
- Customer WiFi — completely separate subnet
- No cross-subnet access without explicit firewall rule

This simple segmentation significantly reduces the blast
radius of any single security incident.

---

*Last updated: May 2026*
*Amir Maharjan — Kathmandu, Nepal*