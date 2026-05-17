
# Network Security

This folder contains network security notes compiled from
two dedicated security modules completed during my academic
studies at ISMT College Kathmandu:

- **Network Security** — Pearson BTEC Level 5 HND in
  Computing (Software Engineering)
- **Advanced Cyber Security (CET324)** — BSc (Hons)
  Computer Systems Engineering — University of Sunderland
  Final Year — Mark: 65

Supplemented by practical experience gained during my
IT Systems Administrator internship at Addon Engineering
Solutions, Kathmandu (September 2024 to February 2025).

---

## Why Network Security Matters to Me

Network security is not an abstract academic interest.
During my internship my wife became the victim of an
online financial scam — a social engineering attack that
exploited the exact vulnerabilities documented in these
notes. That personal experience permanently clarified
my professional direction toward cybersecurity.

Nepal's digital economy is expanding rapidly while
cybersecurity infrastructure and awareness remain
critically underdeveloped. Understanding network security
at an advanced level is essential to my goal of
contributing meaningfully to digital safety in Nepal. 

---

## Contents

| File | Description |
|------|-------------|
| `firewalls-and-ids.md` | Firewall types, stateful inspection, DMZ, IDS vs IPS, detection methods, open source tools |
| `vpn-and-encryption.md` | Symmetric and asymmetric encryption, hashing, TLS handshake, VPN protocols, IPSec |
| `wireless-security.md` | WiFi standards, WEP/WPA/WPA2/WPA3, wireless attacks, evil twin, deauth attacks, best practices |
| `network-security-best-practices.md` | Defence in depth, least privilege, network hardening, patch management, monitoring, incident response |

---

## Key Concepts Covered

**Firewalls and IDS:**
Packet filtering, stateful inspection, application layer
firewalls, NGFW, DMZ architecture, signature-based and
anomaly-based detection, open source tools including
Snort, Suricata, and OSSEC.

**Encryption and VPN:**
AES, RSA, ECC symmetric and asymmetric encryption,
SHA-256 and bcrypt hashing, TLS 1.2 and 1.3 handshake
process, OpenVPN, WireGuard, IPSec transport and
tunnel modes, certificate authorities.

**Wireless Security:**
WiFi 802.11 standards, WPA3 SAE authentication,
evil twin attacks, deauthentication attacks, KRACK
vulnerability, WPS brute force, rogue access points,
public WiFi risks.

**Best Practices:**
NIST Cybersecurity Framework — Identify, Protect,
Detect, Respond, Recover — applied to network security.
Principle of least privilege, network hardening
checklists, patch management process, CVSS scoring,
continuous monitoring, incident response procedures.

---

# Firewalls and Intrusion Detection Systems

Notes from Network Security module — Pearson BTEC Level 5 HND
and Advanced Cyber Security CET324 — BSc Computer Systems Engineering
ISMT College Kathmandu — Amir Maharjan

---

## 1. Firewalls

A firewall is a network security device that monitors and
controls incoming and outgoing network traffic based on
predefined security rules. It establishes a barrier between
trusted internal networks and untrusted external networks.

### Types of Firewalls

#### Packet Filtering Firewall
- Operates at Network layer (Layer 3) and Transport layer (Layer 4)
- Inspects each packet independently
- Filters based on source IP, destination IP, port, protocol
- Stateless — does not track connection state
- Fast but limited intelligence

! Example ACL acting as packet filter on Cisco router
R1(config)# access-list 100 permit tcp 192.168.1.0 0.0.0.255 any eq 443
R1(config)# access-list 100 permit tcp 192.168.1.0 0.0.0.255 any eq 80
R1(config)# access-list 100 deny ip any any log
R1(config)# interface GigabitEthernet0/1
R1(config-if)# ip access-group 100 out

#### Stateful Inspection Firewall
- Tracks the state of network connections
- Maintains state table of active connections
- Can determine if packet is part of established connection
- More intelligent than packet filtering
- Can detect some attacks that packet filtering cannot

**State table example:**
| Source IP | Source Port | Dest IP | Dest Port | Protocol | State |
|-----------|-------------|---------|-----------|----------|-------|
| 192.168.1.5 | 54321 | 8.8.8.8 | 443 | TCP | ESTABLISHED |
| 192.168.1.10 | 54322 | 1.1.1.1 | 80 | TCP | SYN_SENT |

#### Application Layer Firewall (Proxy Firewall)
- Operates at Application layer (Layer 7)
- Understands specific application protocols
- Can inspect content of traffic — not just headers
- Can block specific content, URLs, file types
- Slower than packet filtering but much more intelligent

#### Next Generation Firewall (NGFW)
- Combines traditional firewall with additional security features
- Deep Packet Inspection (DPI) — inspects packet payload
- Integrated Intrusion Prevention System (IPS)
- Application awareness and control
- User identity awareness
- SSL/TLS inspection — can inspect encrypted traffic
- Threat intelligence integration

### Firewall Rules — Best Practices

**Default deny principle:**
- Start with deny all rule
- Explicitly permit only what is needed
- Any traffic not explicitly permitted is denied

**Rule ordering:**
- Rules processed top to bottom
- First matching rule is applied
- Most specific rules at top
- Default deny rule at bottom

**Example rule set for small business:**

Rule 1: Permit HTTPS (443) from LAN to internet
Rule 2: Permit HTTP (80) from LAN to internet
Rule 3: Permit DNS (53) from LAN to DNS servers
Rule 4: Permit established/related connections inbound
Rule 5: Deny all other traffic

### DMZ — Demilitarised Zone
A DMZ is a network segment that sits between the internal
network and the internet — used for servers that need to
be accessible from the internet.

Internet → [External Firewall] → DMZ → [Internal Firewall] → LAN
DMZ contains:
Web servers
Mail servers
DNS servers
VPN concentrators

**Security benefit:**
If a DMZ server is compromised, the internal firewall
prevents the attacker from reaching internal LAN resources.

---

## 2. Intrusion Detection Systems (IDS)

An IDS monitors network traffic or system activity for
suspicious behaviour and generates alerts when threats
are detected.

### IDS vs IPS

| Feature | IDS | IPS |
|---------|-----|-----|
| Action | Detect and alert only | Detect and block |
| Placement | Out of band — copy of traffic | Inline — all traffic passes through |
| Impact on traffic | None | Can cause latency |
| Risk | May miss attacks | May block legitimate traffic |
| Response | Passive | Active |

### Types of IDS

#### Network-based IDS (NIDS)
- Monitors network traffic across entire network segment
- Placed at strategic points — network perimeter, DMZ
- Can detect network-based attacks
- Cannot see encrypted traffic unless decryption configured
- Cannot detect host-based attacks

#### Host-based IDS (HIDS)
- Installed on individual hosts
- Monitors system calls, file changes, log files
- Can detect attacks that NIDS cannot see
- Can monitor encrypted traffic after decryption
- Resource intensive on host

### Detection Methods

#### Signature-based Detection
- Compares traffic against database of known attack signatures
- Very effective against known threats
- Cannot detect unknown or zero-day attacks
- Requires regular signature updates

#### Anomaly-based Detection
- Establishes baseline of normal behaviour
- Alerts when behaviour deviates significantly from baseline
- Can detect unknown attacks
- Higher false positive rate than signature-based
- Requires time to establish accurate baseline

#### Heuristic-based Detection
- Uses rules and algorithms to identify suspicious behaviour
- Combination of signature and anomaly approaches
- Identifies behaviour patterns associated with attacks

### Common IDS/IPS Tools
- **Snort** — open source NIDS — widely used
- **Suricata** — open source — high performance
- **OSSEC** — open source HIDS
- **Zeek (Bro)** — network analysis framework
- **Security Onion** — Linux distribution for security monitoring

---

## 3. Firewall and IDS in Nepal Context

### Small Business Implementation

For a business like Angels Inc the minimum viable
security implementation includes:

**Router with basic ACL (free — already have router):**
- Block inbound connections not initiated from inside
- Restrict outbound to necessary services only
- Log all denied connections

**Host-based firewall (free — built into OS):**
- Windows Defender Firewall on all laptops
- Configured to block unnecessary inbound connections
- Enabled and actively monitored

**Future implementation after Master's degree:**
- Dedicated firewall appliance or pfSense open source firewall
- NIDS using Snort or Suricata
- Centralised logging and alerting
- Regular firewall rule review

### Nepal Cybersecurity Infrastructure
- Most small businesses in Nepal have no dedicated firewall
- ISP-provided routers often have default or weak configurations
- Limited awareness of IDS/IPS solutions
- Growing need for affordable security solutions appropriate
  for Nepal's small business landscape

---

# VPN and Encryption

Notes from Network Security module — Pearson BTEC Level 5 HND
and Advanced Cyber Security CET324 — BSc Computer Systems Engineering
ISMT College Kathmandu — Amir Maharjan

---

## 1. Encryption Fundamentals

Encryption is the process of converting readable data
(plaintext) into an unreadable format (ciphertext) using
an algorithm and a key. Only authorised parties with the
correct key can decrypt and read the data.

### Why Encryption Matters

**Confidentiality:** Encrypted data cannot be read if intercepted
**Integrity:** Encrypted data reveals tampering attempts
**Authentication:** Encryption supports identity verification
**Non-repudiation:** Digital signatures prove sender identity

### Symmetric Encryption

Same key used for both encryption and decryption.

**Advantages:**
- Fast — suitable for large amounts of data
- Low computational overhead
- Simple key management between two known parties

**Disadvantages:**
- Key distribution problem — how to securely share the key
- Scalability — need separate key for each pair of communicators
- If key is compromised all data is compromised

**Common algorithms:**

| Algorithm | Key Size | Status | Notes |
|-----------|----------|--------|-------|
| DES | 56-bit | Broken — do not use | Deprecated |
| 3DES | 112/168-bit | Weak — phase out | Legacy systems |
| AES-128 | 128-bit | Strong | Acceptable |
| AES-256 | 256-bit | Very strong | Recommended |
| ChaCha20 | 256-bit | Very strong | Mobile friendly |

### Asymmetric Encryption

Uses mathematically related key pair — public key and private key.
What one key encrypts only the other can decrypt.

**How it works:**
1. Generate key pair — public key and private key
2. Share public key freely — anyone can have it
3. Keep private key secret — never share
4. Sender encrypts with recipient's public key
5. Only recipient can decrypt with their private key

**Advantages:**
- Solves key distribution problem
- Enables digital signatures
- Scalable — one key pair per user

**Disadvantages:**
- Much slower than symmetric encryption
- Not suitable for large data volumes
- Used for key exchange and signatures — not bulk data

**Common algorithms:**

| Algorithm | Key Size | Use |
|-----------|----------|-----|
| RSA-2048 | 2048-bit | Key exchange, signatures |
| RSA-4096 | 4096-bit | High security key exchange |
| ECC-256 | 256-bit | Efficient — mobile and IoT |
| Ed25519 | 256-bit | Digital signatures |

### Hybrid Encryption

In practice most systems use both:
1. Asymmetric encryption to securely exchange a symmetric key
2. Symmetric encryption for the actual data

**Example — HTTPS:**
1. Browser and server use asymmetric (RSA/ECC) to exchange
   AES session key
2. All subsequent communication encrypted with AES session key
3. Fast bulk encryption with secure key exchange

---

## 2. Hashing

A hash function converts data of any size into a fixed-size
output (hash/digest). It is a one-way function — cannot be
reversed.

### Properties of Secure Hash Functions

- **Deterministic** — same input always produces same output
- **One-way** — cannot derive input from output
- **Avalanche effect** — small change in input produces
  completely different output
- **Collision resistant** — extremely difficult to find two
  inputs with same hash

### Common Hash Algorithms

| Algorithm | Output Size | Status |
|-----------|-------------|--------|
| MD5 | 128-bit | Broken — do not use for security |
| SHA-1 | 160-bit | Weak — deprecated |
| SHA-256 | 256-bit | Strong — recommended |
| SHA-3 | Variable | Strong — newest standard |
| bcrypt | Variable | Strong — designed for passwords |
| Argon2 | Variable | Very strong — password hashing |

### Security Applications of Hashing

**Password storage:**

User creates password: "MyP@ssw0rd"
System hashes: bcrypt("MyP@ssw0rd") = 2b12$abc...xyz
Stores hash — never stores plaintext password
On login: bcrypt.verify("input", stored_hash)

**File integrity:**

SHA256("important_file.pdf") = a3f5b2...
Share file and hash separately
Recipient calculates hash of received file
If hashes match — file not tampered with

**Digital signatures:**

Sender hashes document → encrypts hash with private key
→ attaches encrypted hash (signature) to document
Recipient decrypts signature with sender public key
→ compares with own hash of document
→ match confirms authenticity and integrity

---

## 3. TLS and HTTPS

Transport Layer Security (TLS) is the cryptographic protocol
that secures most internet communications — including HTTPS.

### TLS Handshake Process

Client                          Server
|                               |
|--- ClientHello -------------->|
|    (TLS version, cipher list) |
|                               |
|<-- ServerHello ---------------|
|    (chosen cipher, cert)      |
|                               |
|--- Certificate Verification ->|
|    (validate server cert)     |
|                               |
|--- Key Exchange ------------->|
|    (asymmetric encryption)    |
|                               |
|<-- Session Key Established ---|
|                               |
|=== Encrypted Communication ===|
|    (symmetric AES encryption) |

### TLS Versions

| Version | Status | Notes |
|---------|--------|-------|
| SSL 2.0 | Broken | Never use |
| SSL 3.0 | Broken | Never use |
| TLS 1.0 | Deprecated | Disable |
| TLS 1.1 | Deprecated | Disable |
| TLS 1.2 | Acceptable | Still widely used |
| TLS 1.3 | Recommended | Fastest and most secure |

### SSL Certificates

**Certificate types:**

| Type | Validation | Use Case |
|------|-----------|---------|
| DV (Domain Validated) | Domain ownership only | Personal sites |
| OV (Organisation Validated) | Organisation identity | Business sites |
| EV (Extended Validation) | Thorough vetting | Banking, e-commerce |
| Wildcard | Covers subdomains | *.example.com |

**Certificate Authorities (CAs):**
- DigiCert, Comodo, GlobalSign — commercial CAs
- Let's Encrypt — free automated certificates
- Self-signed — for internal use only

---

## 4. VPN — Virtual Private Network

A VPN creates an encrypted tunnel between a user and
a network — protecting data in transit from interception.

### How VPN Works

User Device → [Encrypted Tunnel] → VPN Server → Internet
Without VPN:
User → ISP (sees all traffic) → Internet
With VPN:
User → ISP (sees only encrypted VPN traffic) → VPN Server → Internet

### VPN Use Cases

**Remote access:**
- Employee working from home connects to company network
- All traffic encrypted between home and office
- User appears to be on company network

**Site to site:**
- Connect two office locations securely over internet
- Replaces expensive dedicated WAN links
- Both sites appear on same network

**Personal privacy:**
- Hide browsing from ISP
- Access geo-restricted content
- Secure browsing on public WiFi

### VPN Protocols

| Protocol | Encryption | Speed | Security | Notes |
|----------|-----------|-------|----------|-------|
| PPTP | Weak | Fast | Poor | Do not use |
| L2TP/IPSec | AES | Medium | Good | Widely supported |
| OpenVPN | AES-256 | Medium | Very good | Open source |
| IKEv2/IPSec | AES | Fast | Very good | Mobile friendly |
| WireGuard | ChaCha20 | Very fast | Excellent | Modern — recommended |

### IPSec VPN Configuration Concepts

IPSec operates in two modes:

**Transport mode:**
- Encrypts only payload — not IP header
- Used for end-to-end communication
- Hosts must both support IPSec

**Tunnel mode:**
- Encrypts entire original packet
- New IP header added
- Used for site-to-site VPNs
- Most common implementation

**IPSec components:**
- **AH (Authentication Header)** — integrity and authentication
- **ESP (Encapsulating Security Payload)** — encryption and auth
- **IKE (Internet Key Exchange)** — key negotiation

---

## 5. Encryption in Nepal Context

### Digital Payment Security

Nepal's digital payment platforms — eSewa, Khalti, FonePay —
use encryption to protect transactions. Understanding
encryption helps users verify they are using services securely:

**Verify HTTPS:**
- Look for padlock icon in browser
- URL starts with https:// not http://
- Check certificate is valid and for correct domain

**Man in the middle risk:**
- Public WiFi — attackers can intercept unencrypted traffic
- Always use HTTPS for financial transactions
- Consider VPN on public WiFi

### Angels Inc Encryption Recommendations

Based on security assessment of Angels Inc:

1. All business accounts accessed via HTTPS only
2. Verify SSL certificates on payment platforms
3. Consider VPN for business operations on mobile data
4. Enable HTTPS on any business website or portal
5. Use encrypted messaging for sensitive business communication

### Importance for Nepal Cybersecurity

Many Nepali small businesses and individuals:
- Do not understand difference between HTTP and HTTPS
- Connect to public WiFi without VPN protection
- Use outdated applications without TLS 1.2 or 1.3
- Do not verify certificate authenticity

Building encryption awareness is a key part of my
cybersecurity mission for Nepal. 
---

# Wireless Network Security

Notes from Network Security module — Pearson BTEC Level 5 HND
ISMT College Kathmandu — Amir Maharjan

---

## 1. Wireless Network Fundamentals

### WiFi Standards

| Standard | Frequency | Max Speed | Range | Notes |
|----------|-----------|-----------|-------|-------|
| 802.11b | 2.4 GHz | 11 Mbps | 35m | Legacy |
| 802.11g | 2.4 GHz | 54 Mbps | 38m | Legacy |
| 802.11n | 2.4/5 GHz | 600 Mbps | 70m | Widely used |
| 802.11ac | 5 GHz | 3.5 Gbps | 35m | Current standard |
| 802.11ax (WiFi 6) | 2.4/5/6 GHz | 9.6 Gbps | 30m | Latest |

### 2.4 GHz vs 5 GHz

| Feature | 2.4 GHz | 5 GHz |
|---------|---------|-------|
| Range | Longer | Shorter |
| Speed | Slower | Faster |
| Interference | More — many devices | Less |
| Wall penetration | Better | Worse |
| Best for | IoT, long range | High speed, close range |

---

## 2. Wireless Security Protocols

### WEP — Wired Equivalent Privacy
- First wireless security protocol — 1999
- Uses RC4 encryption with 40 or 104-bit keys
- Completely broken — crackable in minutes
- Never use under any circumstances

### WPA — WiFi Protected Access
- Replaced WEP in 2003
- Uses TKIP (Temporal Key Integrity Protocol)
- Significantly better than WEP but still vulnerable
- Should be disabled and replaced with WPA2 or WPA3

### WPA2 — WiFi Protected Access 2
- Introduced 2004 — current standard
- Uses AES-CCMP encryption — much stronger than TKIP
- Two modes:
  - **WPA2-Personal (PSK)** — shared password — home/small office
  - **WPA2-Enterprise** — individual authentication — RADIUS server
- KRACK vulnerability discovered 2017 — patch all devices

### WPA3 — WiFi Protected Access 3
- Introduced 2018 — recommended standard
- Improved password-based authentication — SAE (Dragonfly handshake)
- Forward secrecy — past sessions cannot be decrypted if key compromised
- Protects against offline dictionary attacks
- Enhanced Open — encrypts open networks
- 192-bit security mode for enterprise

### Security Protocol Comparison

| Protocol | Encryption | Status | Recommendation |
|----------|-----------|--------|----------------|
| WEP | RC4 40/104-bit | Broken | Never use |
| WPA | TKIP | Weak | Disable |
| WPA2-Personal | AES-128 | Acceptable | Use if WPA3 unavailable |
| WPA2-Enterprise | AES-128 | Good | For organisations |
| WPA3-Personal | AES-192/SAE | Strong | Recommended |
| WPA3-Enterprise | AES-256 | Very strong | For high security |

---

## 3. Wireless Attack Types

### Passive Attacks

**WiFi Sniffing/Eavesdropping:**
- Capturing wireless traffic using tools like Wireshark
- On unencrypted or weakly encrypted networks attacker
  can read all traffic
- Even on encrypted networks metadata visible
- Defence: Strong encryption (WPA3), HTTPS for all web traffic

**Traffic Analysis:**
- Analysing patterns of encrypted traffic
- Can reveal information about communication even without
  decrypting content
- Defence: VPN to hide traffic patterns

### Active Attacks

**Evil Twin Attack:**
- Attacker creates rogue access point with same SSID as
  legitimate network
- Victims connect to attacker's AP instead of real one
- All traffic passes through attacker
- Particularly effective in public places

**Attack scenario:**

Legitimate AP: "CoffeeShop_WiFi" (real)
Evil Twin:     "CoffeeShop_WiFi" (attacker controlled)
Victim connects to evil twin → all traffic intercepted
Attacker can see all unencrypted data
Can perform MitM on HTTPS with SSL stripping

**Defence:**
- Verify network authenticity before connecting
- Use VPN on all public WiFi
- Look for certificate warnings in browser
- Use HTTPS everywhere

**Deauthentication Attack:**
- WiFi management frames not authenticated in WPA2
- Attacker sends fake deauth frames to disconnect clients
- Clients reconnect — attacker captures 4-way handshake
- Handshake used for offline password cracking
- Defence: WPA3 (protects management frames), 802.11w

**WPA2 Handshake Capture and Cracking:**

**Defence:**
- Verify network authenticity before connecting
- Use VPN on all public WiFi
- Look for certificate warnings in browser
- Use HTTPS everywhere

**Deauthentication Attack:**
- WiFi management frames not authenticated in WPA2
- Attacker sends fake deauth frames to disconnect clients
- Clients reconnect — attacker captures 4-way handshake
- Handshake used for offline password cracking
- Defence: WPA3 (protects management frames), 802.11w

**WPA2 Handshake Capture and Cracking:**

Attack steps:
Put wireless adapter in monitor mode
Capture traffic on target network
Send deauth packets to force reconnection
Capture 4-way handshake during reconnection
Run offline dictionary/brute force attack on handshake
Defence:
Use long complex WiFi password (20+ characters)
Use WPA3 — resistant to offline attacks
Monitor for deauth floods

**KRACK Attack (Key Reinstallation Attack):**
- 2017 vulnerability affecting WPA2
- Manipulates cryptographic handshake
- Allows decryption of traffic
- Defence: Patch all devices — most vendors released fixes

**WPS Attack:**
- WiFi Protected Setup PIN brute force
- 8-digit PIN reduced to 11,000 combinations due to design flaw
- Can crack WPS PIN in hours
- Defence: Disable WPS on all routers

### Rogue Access Point:
- Unauthorised AP connected to wired network
- Bypasses wired network security controls
- Employee may install innocently — significant security risk
- Defence: Wireless intrusion detection, 802.1X on wired ports

---

## 4. Wireless Security Best Practices

### Router and AP Configuration

Essential settings:
✓ WPA3 or WPA2-AES encryption — never WEP or WPA-TKIP
✓ Strong unique password — minimum 20 characters
✓ Change default admin credentials immediately
✓ Disable WPS — vulnerable to brute force
✓ Enable firewall on router
✓ Keep firmware updated — check monthly
✓ Disable remote management unless required
✓ Use strong admin password — different from WiFi password
✓ Enable logging
Network segmentation:
✓ Separate guest network for visitors
✓ Separate IoT network for smart devices
✓ Main network for trusted devices only

### Strong WiFi Password Guidelines

Bad password:  "password123"  — dictionary attack in seconds
Better:        "N3p@l2026!"   — stronger but still guessable
Best:          "K@thmandu#7Nepal!Cyber2026" — very strong
Password requirements:
Minimum 20 characters
Mix of uppercase, lowercase, numbers, symbols
Not based on personal information
Unique — not used anywhere else
Changed if suspected compromise

### Public WiFi Safety

Risks of public WiFi:
Unencrypted traffic visible to anyone
Evil twin attacks
Man in the middle attacks
Malicious hotspots
Safe practices:
✓ Use VPN on all public WiFi
✓ Only access HTTPS websites
✓ Avoid financial transactions on public WiFi
✓ Turn off automatic WiFi connection
✓ Forget public networks after use
✓ Use mobile data instead where possible

---

## 5. Wireless Security in Nepal Context

### Common Issues in Nepal

**Weak router configurations:**
- Many homes and businesses use ISP-provided routers
  with default or weak passwords
- Default credentials widely known — admin/admin, admin/password
- WPS often enabled by default — easily exploited

**Public WiFi risks:**
- Widespread use of public WiFi in cafes, hotels, malls
- Most public WiFi unencrypted or weakly encrypted
- Growing use for mobile banking without VPN protection

**Business wireless security:**
- Most small businesses use single WiFi network for
  both business and customer/guest devices
- No network segmentation
- Payment processing on same network as guest devices

### Angels Inc Wireless Recommendations

Based on security assessment:

1. Change router admin password from default immediately
2. Update router firmware to latest version
3. Use WPA2-AES minimum — WPA3 if router supports it
4. Create separate guest network for non-business devices
5. Use strong unique WiFi password — minimum 20 characters
6. Disable WPS
7. Never process payments on public WiFi without VPN
8. Review connected devices monthly

---

# Network Security Best Practices

Notes from Network Security module — Pearson BTEC Level 5 HND
and Advanced Cyber Security CET324 — BSc Computer Systems Engineering
ISMT College Kathmandu — Amir Maharjan

Practical experience from IT Systems Administrator Internship
at Addon Engineering Solutions, Kathmandu (Sept 2024 – Feb 2025)

---

## 1. Defence in Depth

Defence in depth is the principle of implementing multiple
layers of security controls so that if one layer fails
others still provide protection.

Layer 1: Physical security
Layer 2: Network perimeter (firewall)
Layer 3: Network segmentation (VLANs)
Layer 4: Host security (endpoint protection)
Layer 5: Application security
Layer 6: Data security (encryption)
Layer 7: User awareness and training

No single security control is sufficient. An attacker who
bypasses the firewall still faces network segmentation,
host-based controls, application security, and encryption.

---

## 2. Network Security Framework

### Identify
Know what you have before you can protect it:

- **Asset inventory** — every device on the network documented
- **Network diagram** — accurate map of network topology
- **Data classification** — what data exists and sensitivity level
- **Risk assessment** — what threats exist and likelihood

### Protect
Implement controls to prevent attacks:

- Firewall with default deny policy
- Network segmentation with VLANs
- Strong authentication — MFA where possible
- Encryption for data in transit and at rest
- Patch management — keep all systems updated
- Access control — least privilege principle
- Security awareness training

### Detect
Identify attacks that bypass prevention:

- Intrusion Detection System (IDS)
- Security Information and Event Management (SIEM)
- Network traffic monitoring
- Log analysis and correlation
- Vulnerability scanning
- User behaviour analytics

### Respond
Have a plan when attacks occur:

- Incident response plan documented and tested
- Defined roles and responsibilities
- Communication procedures
- Containment procedures
- Evidence preservation
- Recovery procedures

### Recover
Restore normal operations after an incident:

- Backup and recovery procedures tested regularly
- Business continuity plan
- Post-incident review and improvement
- Lessons learned documentation

---

## 3. Principle of Least Privilege

Users and systems should have only the minimum access
required to perform their function — nothing more.

**Examples:**
- Standard user accounts — no admin rights for daily work
- Service accounts — only access to specific resources needed
- Database accounts — read-only where write not required
- Network access — devices only reach resources they need

**Implementation:**

! Cisco ACL implementing least privilege
! Allow HR VLAN to access HR server only
ip access-list extended HR_ACCESS
permit tcp 192.168.10.0 0.0.0.255 host 10.0.0.50 eq 443
permit tcp 192.168.10.0 0.0.0.255 host 10.0.0.50 eq 80
deny ip 192.168.10.0 0.0.0.255 10.0.0.0 0.0.0.255
permit ip 192.168.10.0 0.0.0.255 any

---

## 4. Network Hardening Checklist

### Router Hardening

✓ Change default credentials
✓ Use SSH not Telnet for management
✓ Enable encrypted passwords
✓ Set exec-timeout on all lines
✓ Disable unused services and interfaces
✓ Enable logging
✓ Apply ACLs to restrict management access
✓ Use NTP for accurate timestamps
✓ Enable SNMP v3 only if needed — disable v1 and v2c
✓ Keep IOS updated

### Switch Hardening
✓ Change default credentials
✓ Disable unused ports and assign to unused VLAN
✓ Enable port security on access ports
✓ Disable DTP on access ports
✓ Change native VLAN from VLAN 1
✓ Enable BPDU guard on access ports
✓ Enable Dynamic ARP Inspection
✓ Enable DHCP snooping
✓ Use SSH for management
✓ Enable logging

### Firewall Hardening

✓ Default deny all policy
✓ Only permit necessary traffic
✓ Regular rule review — remove unused rules
✓ Log all denied traffic
✓ Enable intrusion prevention features
✓ Keep firmware updated
✓ Restrict management access to specific IPs
✓ Use encrypted management (HTTPS/SSH)
✓ Regular backup of firewall configuration
✓ Test rules after changes

---

## 5. Patch Management

Unpatched systems are one of the most common causes of
successful attacks. A structured patch management process
is essential.

### Patch Management Process

1. Monitor
Subscribe to vendor security advisories
Monitor CVE databases
Automated vulnerability scanning
2. Evaluate
Assess severity of each vulnerability
Determine which systems are affected
Prioritise by CVSS score and business impact
3. Test
Test patches in non-production environment first
Verify patch does not break functionality
Document testing results
4. Deploy
Deploy critical patches within 24-72 hours
Deploy high patches within 2 weeks
Deploy medium patches within 1 month
Maintain deployment records
5. Verify
Confirm patch applied successfully
Rescan to verify vulnerability remediated
Update asset inventory

### Patch Priority

| CVSS Score | Severity | Target Patch Time |
|------------|----------|------------------|
| 9.0 – 10.0 | Critical | 24 hours |
| 7.0 – 8.9 | High | 72 hours |
| 4.0 – 6.9 | Medium | 2 weeks |
| 0.1 – 3.9 | Low | 1 month |

---

## 6. Network Monitoring

Continuous monitoring is essential — you cannot defend
what you cannot see.

### What to Monitor

**Network traffic:**
- Unusual traffic volumes
- Traffic to/from suspicious destinations
- Protocol anomalies
- Port scanning activity
- Large data transfers

**Authentication:**
- Failed login attempts
- Logins from unusual locations or times
- Account lockouts
- Privilege escalation

**System events:**
- Service starts and stops
- Configuration changes
- New software installation
- File system changes on critical systems

### Monitoring Tools

| Tool | Type | Cost | Use Case |
|------|------|------|---------|
| Wireshark | Packet analyser | Free | Detailed traffic analysis |
| Nagios | Network monitoring | Free/Paid | Device and service monitoring |
| Security Onion | SIEM/IDS | Free | Comprehensive security monitoring |
| Graylog | Log management | Free/Paid | Centralised log analysis |
| PRTG | Network monitoring | Paid | Commercial monitoring |
| Splunk | SIEM | Paid | Enterprise security monitoring |

---

## 7. Incident Response — Network Focus

When a network security incident occurs:

### Immediate Response (0-1 hour)

1. Confirm the incident is real — not false positive
2. Activate incident response team
3. Document everything from this point — timestamps essential
4. Assess scope — what systems affected?
5. Make containment decision:
Isolate affected systems from network
Block suspicious IP addresses at firewall
Disable compromised accounts

### Containment (1-24 hours)

1. Isolate affected network segments using VLANs/ACLs
2. Preserve evidence — forensic image before changes
3. Block attack vectors at firewall and IDS
4. Reset compromised credentials
5. Patch exploited vulnerabilities if possible
6. Continue monitoring for attacker persistence

### Recovery

1. Verify systems are clean before reconnecting
2. Restore from known good backups where needed
3. Apply security patches
4. Reset all credentials on affected systems
5. Gradually restore services with enhanced monitoring
6. Document all actions taken

### Post Incident

1. Conduct lessons learned review within 2 weeks
2. Identify what worked and what failed
3. Update incident response procedures
4. Share relevant information with team
5. Implement improvements to prevent recurrence
6. Update threat intelligence with attack indicators

---

## 8. Professional Experience Application

During my IT Systems Administrator internship at Addon
Engineering Solutions I applied network security best
practices in a real professional environment:

**Daily practices:**
- Monitoring network performance for anomalies
- Managing user access following least privilege principle
- Documenting all configuration changes
- Responding to support tickets with security awareness

**Security incidents encountered:**
- Suspicious login attempts detected in logs
- User accounts requiring password resets after phishing
- Network performance issues investigated for security causes

**Key learning:**
The gap between academic security knowledge and practical
implementation became very clear during the internship.
Theoretical knowledge of firewalls, IDS, and monitoring
is valuable — but practical experience of applying these
in a real environment with real consequences is essential.

This gap directly motivates my pursuit of the Master of Information Technology 
which will further develop my practical cybersecurity skills
in a professional environment.

---

*Last updated: May 2026*
*Amir Maharjan — Kathmandu, Nepal*