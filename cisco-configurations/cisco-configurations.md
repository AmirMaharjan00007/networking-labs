# Cisco Network Configurations

Notes and configurations from Networking and Network Security
modules — Pearson BTEC Level 5 HND in Computing (Software Engineering)
ISMT College Kathmandu — Amir Maharjan

Practical experience from IT Systems Administrator Internship
at Addon Engineering Solutions, Kathmandu (Sept 2024 – Feb 2025)

Tool used: Cisco Packet Tracer

---

## 1. Cisco IOS Basics

Cisco IOS (Internetwork Operating System) is the operating
system used on Cisco routers and switches. Configuration
is done through the Command Line Interface (CLI).

### IOS Modes

| Mode | Prompt | Access | Purpose |
|------|--------|--------|---------|
| User EXEC | Router> | Default on login | Basic monitoring |
| Privileged EXEC | Router# | enable command | Full monitoring and debugging |
| Global Config | Router(config)# | configure terminal | Global device settings |
| Interface Config | Router(config-if)# | interface command | Interface specific settings |
| Line Config | Router(config-line)# | line command | Console and VTY settings |

### Navigating Between Modes

Router> enable                    ! Enter privileged EXEC
Router# configure terminal        ! Enter global config
Router(config)# interface fa0/0   ! Enter interface config
Router(config-if)# exit           ! Back one level
Router(config-if)# end            ! Back to privileged EXEC
Router# disable                   ! Back to user EXEC

---

## 2. Basic Router Configuration

### Initial Setup

Router> enable
Router# configure terminal
! Set hostname
Router(config)# hostname R1
! Set privileged EXEC password (encrypted)
R1(config)# enable secret MyStr0ngP@ss
! Disable DNS lookup (prevents delays on mistyped commands)
R1(config)# no ip domain-lookup
! Set login banner
R1(config)# banner motd #
Authorised access only. Unauthorised access is prohibited.

! Save configuration
R1(config)# end
R1# write memory
! or
R1# copy running-config startup-config

### Console Security

R1(config)# line console 0
R1(config-line)# password C0ns0leP@ss
R1(config-line)# login
R1(config-line)# exec-timeout 5 0    ! Timeout after 5 minutes
R1(config-line)# exit

### VTY Lines Security (Remote Access)

R1(config)# line vty 0 4             ! Configure 5 VTY lines
R1(config-line)# password VtyP@ss
R1(config-line)# login
R1(config-line)# transport input ssh  ! Allow SSH only — not Telnet
R1(config-line)# exec-timeout 5 0
R1(config-line)# exit

### Configure SSH (More Secure than Telnet)

R1(config)# ip domain-name addonsolutions.local
R1(config)# crypto key generate rsa modulus 2048
R1(config)# ip ssh version 2
R1(config)# username admin privilege 15 secret Adm1nP@ss
R1(config)# line vty 0 4
R1(config-line)# login local
R1(config-line)# transport input ssh

---

## 3. Interface Configuration

### Configure Router Interface

R1(config)# interface GigabitEthernet0/0
R1(config-if)# description LAN Connection to Switch
R1(config-if)# ip address 192.168.1.1 255.255.255.0
R1(config-if)# no shutdown              ! Enable the interface
R1(config-if)# exit
R1(config)# interface GigabitEthernet0/1
R1(config-if)# description WAN Connection to ISP
R1(config-if)# ip address 203.0.113.1 255.255.255.252
R1(config-if)# no shutdown
R1(config-if)# exit

### Verify Interface Status

R1# show interfaces
R1# show interfaces GigabitEthernet0/0
R1# show ip interface brief          ! Quick summary of all interfaces
R1# show ip interface GigabitEthernet0/0

---

## 4. Routing Configuration

### Static Routing

! Syntax: ip route [destination network] [subnet mask] [next hop]
R1(config)# ip route 10.0.0.0 255.0.0.0 192.168.1.254
R1(config)# ip route 0.0.0.0 0.0.0.0 203.0.113.2  ! Default route
! Verify routing table
R1# show ip route

### RIP — Routing Information Protocol

R1(config)# router rip
R1(config-router)# version 2
R1(config-router)# network 192.168.1.0
R1(config-router)# network 10.0.0.0
R1(config-router)# no auto-summary    ! Disable auto summarisation
R1(config-router)# exit
! Verify RIP
R1# show ip protocols
R1# show ip route rip

### OSPF — Open Shortest Path First

R1(config)# router ospf 1             ! Process ID 1
R1(config-router)# router-id 1.1.1.1 ! Unique router ID
R1(config-router)# network 192.168.1.0 0.0.0.255 area 0
R1(config-router)# network 10.0.0.0 0.255.255.255 area 0
R1(config-router)# exit
! Verify OSPF
R1# show ip ospf neighbor
R1# show ip ospf database
R1# show ip route ospf

---

## 5. Switch Configuration

### Basic Switch Setup

Switch> enable
Switch# configure terminal
Switch(config)# hostname SW1
! Secure privileged EXEC
SW1(config)# enable secret Sw1tch@Pass
! Management IP address on VLAN 1
SW1(config)# interface vlan 1
SW1(config-if)# ip address 192.168.1.10 255.255.255.0
SW1(config-if)# no shutdown
SW1(config-if)# exit
! Default gateway
SW1(config)# ip default-gateway 192.168.1.1
! Save
SW1# write memory

### Port Security

Port security limits which MAC addresses can connect to
a switch port — prevents unauthorised devices connecting
to network.

SW1(config)# interface FastEthernet0/1
SW1(config-if)# switchport mode access
SW1(config-if)# switchport port-security
SW1(config-if)# switchport port-security maximum 1
SW1(config-if)# switchport port-security mac-address sticky
SW1(config-if)# switchport port-security violation shutdown
SW1(config-if)# exit
! Verify port security
SW1# show port-security
SW1# show port-security interface FastEthernet0/1

**Violation modes:**
- **Shutdown** — port disabled when violation occurs (recommended)
- **Restrict** — drops violating frames, logs and counts violation
- **Protect** — drops violating frames silently

---

## 6. VLAN Configuration

VLANs (Virtual Local Area Networks) logically segment a
network — devices on different VLANs cannot communicate
without a router even if on same physical switch.

### Create VLANs

SW1(config)# vlan 10
SW1(config-vlan)# name Management
SW1(config-vlan)# exit
SW1(config)# vlan 20
SW1(config-vlan)# name Sales
SW1(config-vlan)# exit
SW1(config)# vlan 30
SW1(config-vlan)# name Guest
SW1(config-vlan)# exit
! Verify VLANs
SW1# show vlan brief

### Assign Ports to VLANs

! Access port — connects end devices
SW1(config)# interface FastEthernet0/1
SW1(config-if)# switchport mode access
SW1(config-if)# switchport access vlan 10
SW1(config-if)# exit
SW1(config)# interface FastEthernet0/2
SW1(config-if)# switchport mode access
SW1(config-if)# switchport access vlan 20
SW1(config-if)# exit

### Trunk Port Configuration

Trunk ports carry traffic for multiple VLANs between
switches or between switch and router.

! Configure trunk port
SW1(config)# interface GigabitEthernet0/1
SW1(config-if)# switchport mode trunk
SW1(config-if)# switchport trunk allowed vlan 10,20,30
SW1(config-if)# switchport trunk native vlan 99
SW1(config-if)# exit
! Verify trunk
SW1# show interfaces trunk

### Inter-VLAN Routing — Router on a Stick

Allows communication between VLANs using subinterfaces
on a router.

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

---

## 7. Access Control Lists (ACLs)

ACLs filter network traffic based on defined rules —
permit or deny traffic based on IP address, protocol,
and port number.

### Standard ACLs — Filter by Source IP Only

! Permit only 192.168.10.0/24 network
R1(config)# access-list 10 permit 192.168.10.0 0.0.0.255
R1(config)# access-list 10 deny any
! Apply to interface — inbound traffic
R1(config)# interface GigabitEthernet0/1
R1(config-if)# ip access-group 10 in
R1(config-if)# exit
! Verify ACL
R1# show access-lists
R1# show ip interface GigabitEthernet0/1

### Extended ACLs — Filter by Source, Destination, Protocol, Port

! Permit HTTP traffic from LAN to internet
R1(config)# access-list 100 permit tcp 192.168.1.0 0.0.0.255 any eq 80
R1(config)# access-list 100 permit tcp 192.168.1.0 0.0.0.255 any eq 443
! Deny all other traffic
R1(config)# access-list 100 deny ip any any
! Apply outbound on WAN interface
R1(config)# interface GigabitEthernet0/1
R1(config-if)# ip access-group 100 out
R1(config-if)# exit

### Named ACLs — More Readable

R1(config)# ip access-list extended BLOCK_GUEST
R1(config-ext-nacl)# deny ip 192.168.30.0 0.0.0.255 192.168.10.0 0.0.0.255
R1(config-ext-nacl)# deny ip 192.168.30.0 0.0.0.255 192.168.20.0 0.0.0.255
R1(config-ext-nacl)# permit ip any any
R1(config-ext-nacl)# exit
R1(config)# interface GigabitEthernet0/0.30
R1(config-if)# ip access-group BLOCK_GUEST in

*Security application: This ACL prevents guest VLAN devices
from accessing management or sales VLANs — directly applicable
to Angels Inc network segmentation recommendation.*

---

## 8. NAT Configuration

### PAT — Port Address Translation (Most Common)

Allows multiple internal devices to share one public IP.

! Define inside and outside interfaces
R1(config)# interface GigabitEthernet0/0
R1(config-if)# ip nat inside
R1(config-if)# exit
R1(config)# interface GigabitEthernet0/1
R1(config-if)# ip nat outside
R1(config-if)# exit
! Create ACL defining which hosts to translate
R1(config)# access-list 1 permit 192.168.1.0 0.0.0.255
! Configure PAT
R1(config)# ip nat inside source list 1 interface GigabitEthernet0/1 overload
! Verify NAT
R1# show ip nat translations
R1# show ip nat statistics

---

## 9. DHCP Server Configuration on Router

! Exclude addresses reserved for static assignment
R1(config)# ip dhcp excluded-address 192.168.1.1 192.168.1.20
! Create DHCP pool
R1(config)# ip dhcp pool LAN_POOL
R1(dhcp-config)# network 192.168.1.0 255.255.255.0
R1(dhcp-config)# default-router 192.168.1.1
R1(dhcp-config)# dns-server 8.8.8.8 8.8.4.4
R1(dhcp-config)# lease 7                ! 7 day lease
R1(dhcp-config)# exit
! Verify DHCP
R1# show ip dhcp binding
R1# show ip dhcp pool

---

## 10. Useful Verification Commands

### Router Verification

R1# show version                    ! IOS version and hardware info
R1# show running-config             ! Current active configuration
R1# show startup-config             ! Saved configuration
R1# show ip interface brief         ! All interfaces summary
R1# show ip route                   ! Routing table
R1# show ip protocols               ! Routing protocols running
R1# show cdp neighbors              ! Connected Cisco devices
R1# show processes cpu              ! CPU utilisation
R1# show memory                     ! Memory usage

### Switch Verification

SW1# show vlan brief                ! VLAN summary
SW1# show interfaces trunk          ! Trunk port status
SW1# show mac address-table         ! MAC address table
SW1# show port-security             ! Port security summary
SW1# show spanning-tree             ! STP status

### Connectivity Testing

R1# ping 192.168.1.100              ! Basic ping
R1# ping 192.168.1.100 repeat 100  ! Extended ping — 100 packets
R1# traceroute 192.168.1.100        ! Trace route to destination
R1# telnet 192.168.1.100            ! Test connectivity (insecure)

---

## 11. Security Best Practices — Cisco Devices

Based on academic study and professional internship experience:

### Passwords and Authentication
- Always use `enable secret` not `enable password` — secret
  uses MD5 hashing
- Use SSH not Telnet — Telnet sends data in plaintext
- Use local username and password or RADIUS/TACACS+ for
  authentication
- Set exec-timeout on all lines — prevent unattended sessions

### Disable Unused Services

R1(config)# no cdp run              ! Disable CDP if not needed
R1(config)# no ip http server       ! Disable HTTP management
R1(config)# no ip http secure-server ! Disable HTTPS management
R1(config)# no service finger
R1(config)# no service tcp-small-servers
R1(config)# no service udp-small-servers

### Logging

R1(config)# logging on
R1(config)# logging buffered 16384
R1(config)# logging trap informational
R1(config)# service timestamps log datetime msec

### Encrypt Passwords in Config

R1(config)# service password-encryption  ! Encrypts all plaintext passwords

---

## 12. Connection to Professional Experience

During my IT Systems Administrator internship at Addon
Engineering Solutions, Kathmandu, I applied Cisco knowledge
in a real professional environment:

- Used CLI knowledge to understand and document existing
  network configurations
- Applied understanding of VLANs to comprehend network
  segmentation in office environment
- Used verification commands daily for network monitoring
  and troubleshooting
- Applied ACL knowledge to understand firewall rules
  protecting network resources
- Used routing knowledge to diagnose connectivity issues
  between network segments

The Cisco Packet Tracer simulations completed during my
BTEC HND provided a strong practical foundation that
translated directly to working with real network equipment
during the internship.

---

*Last updated: May 2026*
*Amir Maharjan — Kathmandu, Nepal*
