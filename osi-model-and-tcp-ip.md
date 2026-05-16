# OSI Model and TCP/IP

Notes from Networking module — Pearson BTEC Level 5 HND
in Computing (Software Engineering)
ISMT College Kathmandu — Amir Maharjan

---

## 1. The OSI Model

The Open Systems Interconnection (OSI) model is a conceptual
framework that standardises how different network systems
communicate with each other. Developed by the International
Organisation for Standardisation (ISO) in 1984, it divides
network communication into seven distinct layers.

Understanding the OSI model is fundamental to networking and
cybersecurity because:
- It helps identify exactly where in the communication process
  a problem or attack is occurring
- Security controls are applied at specific layers
- Different attack types target different layers
- Troubleshooting becomes systematic rather than guesswork

---

## 2. The Seven Layers — Detailed

### Layer 7 — Application Layer
**Function:** Provides network services directly to end user
applications. This is the layer users interact with directly.

**What happens here:**
- User requests a web page — browser sends HTTP request
- User sends email — email client uses SMTP
- User transfers file — FTP client initiates connection
- Domain name resolution — DNS query sent

**Key protocols:**
- HTTP/HTTPS — web browsing
- FTP/SFTP — file transfer
- SMTP/POP3/IMAP — email
- DNS — domain name resolution
- DHCP — automatic IP assignment
- SSH — secure remote access
- Telnet — remote access (insecure — avoid)

**Security threats at this layer:**
- Phishing attacks
- SQL injection
- Cross-site scripting (XSS)
- Application layer DDoS
- DNS poisoning

**Security controls:**
- Web application firewalls (WAF)
- Input validation
- HTTPS encryption
- Application security testing

---

### Layer 6 — Presentation Layer
**Function:** Translates data between application format and
network format. Handles encryption, compression, and encoding.

**What happens here:**
- Data is encrypted before transmission (SSL/TLS)
- Data is compressed to reduce size
- Data is encoded into standard format
- Data is decrypted when received

**Key protocols and standards:**
- SSL/TLS — encryption
- JPEG, PNG, GIF — image formats
- ASCII, Unicode — text encoding
- MPEG — video compression

**Security relevance:**
- SSL/TLS stripping attacks occur at this layer
- Encryption implementation vulnerabilities
- Certificate validation

---

### Layer 5 — Session Layer
**Function:** Manages sessions — establishing, maintaining,
and terminating connections between applications.

**What happens here:**
- Login sessions established and tracked
- Data synchronisation checkpoints created
- Sessions terminated when complete
- Session restoration after interruption

**Key protocols:**
- NetBIOS — network basic input/output system
- RPC — remote procedure call
- SQL sessions
- Authentication protocols

**Security relevance:**
- Session hijacking attacks target this layer
- Session token theft
- Man-in-the-middle attacks

---

### Layer 4 — Transport Layer
**Function:** Provides end-to-end communication between
applications. Handles error recovery, flow control, and
data segmentation.

**What happens here:**
- Data broken into segments
- Port numbers assigned to identify applications
- TCP — reliable connection established
- UDP — data sent without connection
- Error checking and retransmission

**Key protocols:**

**TCP — Transmission Control Protocol**
- Connection-oriented — three way handshake before data transfer
- Reliable — guarantees delivery and correct order
- Flow control — prevents overwhelming receiver
- Used for: web browsing, email, file transfer
- Three way handshake: SYN → SYN-ACK → ACK

**UDP — User Datagram Protocol**
- Connectionless — no handshake
- Unreliable — no delivery guarantee
- Fast — no overhead of connection establishment
- Used for: video streaming, gaming, DNS, VoIP

**Port Numbers:**
| Port | Protocol | Service |
|------|----------|---------|
| 20/21 | TCP | FTP |
| 22 | TCP | SSH |
| 23 | TCP | Telnet |
| 25 | TCP | SMTP |
| 53 | TCP/UDP | DNS |
| 80 | TCP | HTTP |
| 443 | TCP | HTTPS |
| 3389 | TCP | RDP |
| 3306 | TCP | MySQL |

**Security threats at this layer:**
- Port scanning — identifying open ports
- SYN flood attack — overwhelming TCP handshake
- UDP flood attack
- Port hijacking

**Security controls:**
- Firewall rules based on port numbers
- Closing unnecessary open ports
- Rate limiting connections

---

### Layer 3 — Network Layer
**Function:** Logical addressing and routing — determines
the best path for data to travel from source to destination
across multiple networks.

**What happens here:**
- IP addresses assigned to identify source and destination
- Routing decisions made
- Packets forwarded between networks
- Logical addressing separate from physical addressing

**Key protocols:**
- IP (IPv4 and IPv6) — logical addressing
- ICMP — error reporting and diagnostics (ping uses ICMP)
- ARP — maps IP addresses to MAC addresses
- OSPF, RIP, BGP — routing protocols

**IPv4 vs IPv6:**

| Feature | IPv4 | IPv6 |
|---------|------|------|
| Address length | 32-bit | 128-bit |
| Format | Dotted decimal — 192.168.1.1 | Hexadecimal — 2001:db8::1 |
| Address space | ~4.3 billion | ~340 undecillion |
| Security | Optional (IPSec) | Built-in (IPSec) |
| Status | Current standard | Gradually replacing IPv4 |

**Security threats at this layer:**
- IP spoofing — forging source IP address
- ARP poisoning — corrupting ARP cache
- ICMP attacks — ping flood, smurf attack
- Routing protocol attacks

**Security controls:**
- Ingress and egress filtering
- Dynamic ARP Inspection
- IPSec for encryption at network layer
- Router access control lists

---

### Layer 2 — Data Link Layer
**Function:** Physical addressing and reliable transmission
between directly connected devices. Handles error detection
at the physical level.

**What happens here:**
- MAC addresses used to identify devices on local network
- Frames created from packets
- Error detection using CRC (Cyclic Redundancy Check)
- Access to physical media controlled

**Key protocols:**
- Ethernet — most common wired networking standard
- WiFi (802.11) — wireless networking
- PPP — point-to-point protocol
- ARP — address resolution protocol

**MAC Addresses:**
- 48-bit hardware address burned into network interface card
- Format: 00:1A:2B:3C:4D:5E
- First 24 bits — manufacturer identifier (OUI)
- Last 24 bits — unique device identifier
- Used for communication within local network segment

**Switches operate at Layer 2:**
- Learn MAC addresses of connected devices
- Forward frames only to correct port
- Maintain MAC address table

**Security threats at this layer:**
- MAC spoofing — forging MAC address
- ARP poisoning
- VLAN hopping
- Switch flooding attacks

**Security controls:**
- Port security on switches — limit MAC addresses per port
- Dynamic ARP Inspection
- VLAN segmentation
- 802.1X authentication

---

### Layer 1 — Physical Layer
**Function:** Physical transmission of raw bits over
physical medium. Defines electrical, mechanical, and
procedural specifications.

**What happens here:**
- Bits converted to electrical signals (copper cable)
- Bits converted to light pulses (fibre optic)
- Bits converted to radio waves (wireless)
- Physical connectors and cable standards defined

**Physical media types:**
- Twisted pair copper cable — Cat5e, Cat6, Cat6a
- Fibre optic cable — single mode, multimode
- Wireless — 2.4GHz, 5GHz radio frequencies
- Coaxial cable — older installations

**Physical devices at Layer 1:**
- Hubs — broadcast to all ports (largely obsolete)
- Repeaters — extend signal range
- Network interface cards (NICs)
- Cables and connectors

**Security threats at this layer:**
- Physical wiretapping
- Hardware keyloggers
- Rogue wireless access points
- Cable cutting or damage

**Security controls:**
- Physical access controls
- Cable management and documentation
- Locked network closets
- Tamper-evident seals on hardware

---

## 3. Data Encapsulation

As data travels down the OSI layers from sender, each layer
adds its own header (and sometimes trailer) — this is called
encapsulation.
Application data
↓ + Application header
Segment (Transport layer adds port numbers)
↓ + Transport header
Packet (Network layer adds IP addresses)
↓ + Network header
Frame (Data Link layer adds MAC addresses)
↓ + Data Link header + trailer
Bits (Physical layer converts to signals)

At the receiver the process reverses — each layer removes
its header and passes data up — called de-encapsulation.

**Why this matters for security:**
- Deep packet inspection analyses headers at multiple layers
- Firewalls filter based on headers at layers 3 and 4
- Intrusion detection systems analyse patterns across layers
- Encryption at layer 6 protects payload but headers may
  still be visible

---

## 4. TCP/IP Model

The TCP/IP model is the practical model used on the internet.
It has four layers that map to the OSI seven layers:

| TCP/IP Layer | OSI Equivalent | Key Protocols |
|-------------|----------------|---------------|
| Application | Layers 5, 6, 7 | HTTP, FTP, DNS, SMTP, SSH |
| Transport | Layer 4 | TCP, UDP |
| Internet | Layer 3 | IP, ICMP, ARP |
| Network Access | Layers 1, 2 | Ethernet, WiFi, MAC |

---

## 5. Key Protocols — Detailed

### DNS — Domain Name System
- Translates human-readable domain names to IP addresses
- Example: waikato.ac.nz → 130.217.x.x
- Hierarchical distributed database
- DNS query types: A, AAAA, MX, CNAME, NS, PTR

**DNS security issues:**
- DNS cache poisoning — injecting false records
- DNS amplification DDoS — using DNS for traffic amplification
- DNS tunnelling — hiding data in DNS queries
- typosquatting — registering similar domain names

**DNS security solutions:**
- DNSSEC — digitally signs DNS records
- DNS over HTTPS (DoH) — encrypts DNS queries
- DNS over TLS (DoT) — encrypts DNS queries
- Monitoring DNS queries for anomalies

### DHCP — Dynamic Host Configuration Protocol
- Automatically assigns IP addresses to devices
- Eliminates manual IP configuration
- Assigns: IP address, subnet mask, default gateway, DNS server

**DHCP security issues:**
- DHCP starvation — exhausting IP address pool
- Rogue DHCP server — attacker distributes false network config
- Man in the middle via rogue DHCP

**DHCP security solutions:**
- DHCP snooping on switches
- Static IP for critical devices
- Monitor for rogue DHCP servers

### HTTP vs HTTPS
| Feature | HTTP | HTTPS |
|---------|------|-------|
| Encryption | None | TLS/SSL encryption |
| Port | 80 | 443 |
| Data visibility | Visible in transit | Encrypted in transit |
| Certificate | Not required | Required |
| Use | Should be avoided | Standard for all websites |

---

## 6. Subnetting Basics

Subnetting divides a large network into smaller subnetworks
for better management, performance, and security.

### CIDR Notation
- /24 = 255.255.255.0 = 256 addresses (254 usable)
- /25 = 255.255.255.128 = 128 addresses (126 usable)
- /26 = 255.255.255.192 = 64 addresses (62 usable)
- /16 = 255.255.0.0 = 65,536 addresses
- /8 = 255.0.0.0 = 16,777,216 addresses

### Private IP Address Ranges
Reserved for internal network use — not routable on internet:
- 10.0.0.0/8 — Class A private
- 172.16.0.0/12 — Class B private
- 192.168.0.0/16 — Class C private (most common home/small office)

### Why Subnetting Matters for Security
- Network segmentation through subnetting limits attack spread
- Critical systems placed on isolated subnets
- Firewall rules applied between subnets
- Attacker who compromises one subnet cannot automatically
  reach devices on other subnets

---

## 7. Practical Application — Internship Experience

During my IT Systems Administrator internship at Addon Engineering
Solutions I applied OSI and TCP/IP knowledge directly:

**Troubleshooting using OSI model:**
When network issues occurred I used the OSI model systematically:
- Layer 1 first — is the cable connected? Is the port light on?
- Layer 2 — is the switch seeing the MAC address?
- Layer 3 — is the IP address correctly assigned?
- Layer 4 — is the correct port open and accessible?
- Layer 7 — is the application correctly configured?

This systematic approach significantly reduced troubleshooting
time compared to random trial and error.

**Protocol knowledge in practice:**
- DNS issues frequently caused connectivity problems that appeared
  as Layer 7 application failures but were actually Layer 3
  protocol issues
- Understanding TCP three way handshake helped diagnose connection
  timeout issues
- Port knowledge was essential for firewall rule configuration
  and access control management

---

*Last updated: May 2026*
*Amir Maharjan — Kathmandu, Nepal*

