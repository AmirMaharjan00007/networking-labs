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
