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
