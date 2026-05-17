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
