# VLANs and Switching

Notes from Networking and Network Security modules
Pearson BTEC Level 5 HND in Computing (Software Engineering),
Bachelors in Computer Systems Engineering,
ISMT College Kathmandu — Amir Maharjan

---

## 1. What is a Switch?

A network switch is a Layer 2 device that connects devices
within a local area network (LAN). Unlike a hub which
broadcasts to all ports, a switch learns MAC addresses and
forwards frames only to the correct destination port.

### How a Switch Works

1. Device sends frame onto network
2. Switch receives frame and reads source MAC address
3. Switch records MAC address and port in MAC address table
4. Switch looks up destination MAC in table
5. If found — forwards frame to correct port only
6. If not found — floods frame to all ports except source

### Switch vs Hub vs Router

| Device | Layer | Intelligence | Broadcast Domain | Collision Domain |
|--------|-------|-------------|-----------------|-----------------|
| Hub | 1 | None — broadcasts everything | One | One shared |
| Switch | 2 | MAC address table | One | Per port |
| Router | 3 | Routing table | Per interface | Per interface |

---

## 2. MAC Address Table

The MAC address table (also called CAM table) maps MAC
addresses to switch ports.

SW1# show mac address-table
! Output shows:
! VLAN  MAC Address       Type    Ports
! 1     0050.7966.6800    DYNAMIC Fa0/1
! 1     0050.7966.6801    DYNAMIC Fa0/2

### MAC Address Table Security Issues

**MAC Flooding Attack:**
- Attacker sends thousands of frames with fake MAC addresses
- Fills MAC address table — switch cannot add legitimate entries
- Switch reverts to hub behaviour — floods all frames
- Attacker can capture all network traffic

**Defence:**
- Port security — limit MAC addresses per port
- Monitor MAC address table size
- Use managed switches with security features

---

## 3. VLANs — Virtual Local Area Networks

A VLAN is a logical grouping of network devices regardless
of their physical location. Devices in the same VLAN
communicate as if on the same physical network even if
they are on different switches.

### Why Use VLANs?

**Security:**
- Separates sensitive traffic from general traffic
- Devices on different VLANs cannot communicate without
  going through a router — where access control can be applied
- Limits broadcast domains — reduces attack surface
- Isolates compromised devices from rest of network

**Performance:**
- Reduces unnecessary broadcast traffic
- Smaller broadcast domains mean better performance
- Traffic stays within VLAN unless routing required

**Management:**
- Group devices logically by function not location
- Easier to apply security policies per group
- Flexible — move devices between VLANs without physical changes

### VLAN Types

| VLAN Type | Description |
|-----------|-------------|
| Data VLAN | Carries user data traffic |
| Management VLAN | For switch management traffic — SSH, SNMP |
| Native VLAN | Untagged traffic on trunk port — change from default VLAN 1 |
| Voice VLAN | Dedicated for VoIP traffic — requires QoS |

---

## 4. VLAN Configuration

### Create and Name VLANs

SW1(config)# vlan 10
SW1(config-vlan)# name Management
SW1(config-vlan)# exit
SW1(config)# vlan 20
SW1(config-vlan)# name Employees
SW1(config-vlan)# exit
SW1(config)# vlan 30
SW1(config-vlan)# name Guest
SW1(config-vlan)# exit
SW1(config)# vlan 99
SW1(config-vlan)# name Native
SW1(config-vlan)# exit
SW1# show vlan brief

### Access Ports — Connect End Devices

! Assign port to VLAN
SW1(config)# interface range FastEthernet0/1-5
SW1(config-if-range)# switchport mode access
SW1(config-if-range)# switchport access vlan 20
SW1(config-if-range)# exit
SW1(config)# interface range FastEthernet0/6-10
SW1(config-if-range)# switchport mode access
SW1(config-if-range)# switchport access vlan 30
SW1(config-if-range)# exit

### Trunk Ports — Carry Multiple VLANs

SW1(config)# interface GigabitEthernet0/1
SW1(config-if)# switchport mode trunk
SW1(config-if)# switchport trunk native vlan 99
SW1(config-if)# switchport trunk allowed vlan 10,20,30,99
SW1(config-if)# exit
SW1# show interfaces trunk

---

## 5. VLAN Security

### VLAN Hopping Attacks

**Switch Spoofing:**
- Attacker configures their device to act as a switch
- Negotiates trunk link with switch using DTP
- Gains access to all VLANs on trunk

**Defence:**

! Disable DTP on all access ports
SW1(config)# interface range FastEthernet0/1-24
SW1(config-if-range)# switchport mode access
SW1(config-if-range)# switchport nonegotiate
SW1(config-if-range)# exit

**Double Tagging Attack:**
- Attacker on native VLAN sends double-tagged frames
- Outer tag matches native VLAN — stripped by first switch
- Inner tag carries attacker's target VLAN
- Frame forwarded to target VLAN

**Defence:**

! Change native VLAN from default VLAN 1
SW1(config)# interface GigabitEthernet0/1
SW1(config-if)# switchport trunk native vlan 99
! Ensure no user devices on native VLAN 99

### Unused Port Security

! Shutdown all unused ports
SW1(config)# interface range FastEthernet0/11-24
SW1(config-if-range)# shutdown
SW1(config-if-range)# switchport mode access
SW1(config-if-range)# switchport access vlan 999
SW1(config-if-range)# exit

---

## 6. Spanning Tree Protocol (STP)

STP prevents switching loops in networks with redundant
paths. Without STP, broadcast frames would loop forever
causing a broadcast storm.

### STP Port States

| State | Duration | Description |
|-------|----------|-------------|
| Blocking | 20 seconds | Receives BPDUs only — no forwarding |
| Listening | 15 seconds | Participating in STP election |
| Learning | 15 seconds | Learning MAC addresses |
| Forwarding | Ongoing | Normal operation — forwards frames |
| Disabled | N/A | Administratively shutdown |

### STP Verification

SW1# show spanning-tree
SW1# show spanning-tree vlan 10
SW1# show spanning-tree summary

### Rapid STP (RSTP — 802.1w)
- Faster convergence than original STP
- Convergence in seconds rather than 50 seconds
- Recommended over original STP

---

## 7. Inter-VLAN Routing

Devices on different VLANs cannot communicate by default.
Inter-VLAN routing allows controlled communication between
VLANs through a router or Layer 3 switch.

### Method 1 — Router on a Stick

Single physical connection between router and switch
using subinterfaces — one per VLAN.

! Switch trunk port to router
SW1(config)# interface GigabitEthernet0/2
SW1(config-if)# switchport mode trunk
SW1(config-if)# switchport trunk allowed vlan 10,20,30
SW1(config-if)# exit
! Router subinterfaces
R1(config)# interface GigabitEthernet0/0
R1(config-if)# no shutdown
R1(config-if)# exit
R1(config)# interface GigabitEthernet0/0.10
R1(config-subif)# encapsulation dot1Q 10
R1(config-subif)# ip address 192.168.10.1 255.255.255.0
R1(config-subif)# exit
R1(config)# interface GigabitEthernet0/0.20
R1(config-subif)# encapsulation dot1Q 20
R1(config-subif)# ip address 192.168.20.1 255.255.255.0
R1(config-subif)# exit
R1(config)# interface GigabitEthernet0/0.30
R1(config-subif)# encapsulation dot1Q 30
R1(config-subif)# ip address 192.168.30.1 255.255.255.0
R1(config-subif)# exit

### Method 2 — Layer 3 Switch

! Enable IP routing on Layer 3 switch
SW1(config)# ip routing
! Create SVI (Switched Virtual Interface) per VLAN
SW1(config)# interface vlan 10
SW1(config-if)# ip address 192.168.10.1 255.255.255.0
SW1(config-if)# no shutdown
SW1(config-if)# exit
SW1(config)# interface vlan 20
SW1(config-if)# ip address 192.168.20.1 255.255.255.0
SW1(config-if)# no shutdown
SW1(config-if)# exit

---

## 8. Real World Application — Angels Inc

Recommended VLAN design for Angels Inc retail business:

| VLAN ID | Name | Devices | Subnet |
|---------|------|---------|--------|
| 10 | Management | Owner laptops | 192.168.10.0/30 |
| 20 | Payment | eSewa/FonePay devices | 192.168.20.0/30 |
| 30 | Guest | Customer WiFi | 192.168.30.0/26 |
| 99 | Native | Trunk native VLAN — no devices | N/A |

**ACL between VLANs:**
- Guest VLAN 30 cannot access Management VLAN 10
- Guest VLAN 30 cannot access Payment VLAN 20
- Management VLAN 10 can access Payment VLAN 20
- All VLANs can access internet

This segmentation directly addresses vulnerability 5
identified in the Angels Inc Security Assessment —
ensuring payment processing devices are isolated from
guest network traffic.

---

*Last updated: May 2026*
*Amir Maharjan — Kathmandu, Nepal*
