### 1. Basic Configuration on All Devices

```bash
enable secret cisco
username cisco secret cisco
line console 0
 login local
 exec-timeout 30
 logging synchronous
line vty 0 4
 password secureshell
 login
 transport input ssh
ip domain-name ccna.local
crypto key generate rsa
```

### 2. VTP and VLANs

**Office 1 - Barcelona**:

```bash
vtp version 2
vtp mode server
vtp domain of1
vtp password of1
vlan 10,20,30,99
```

**Office 2 - Madrid**:

```bash
vtp version 2
vtp mode server
vtp domain of2
vtp password of2
vlan 11,21,31,99
```

> ⚠️ Note: Remember to set other switches in the network as VTP clients to propagate the VLAN database.

```bash
vtp version 2
vtp mode client
vtp domain xxx
vtp password xxxx
```

### 3. EtherChannel

**L2 EtherChannel between DSW ↔ ASW**:

```bash
interface range gi1/0/2 - 4
 channel-group 1 mode desirable
interface port-channel 1
 switchport mode trunk
```

**L3 EtherChannel between DSW-A1 ↔ DSW-A2:**

```bash
interface range gi1/0/5 - 6
 no switchport
 channel-group 2 mode desirable
interface port-channel 2
 no switchport
 ip address x.x.x.x y.y.y.y
```

### 4. Inter-VLAN Routing

**Office 1 (SVIs on Distribution Switches):**

```bash
interface vlan 10
 ip address 192.168.10.2 255.255.255.0
 standby 1 ip 192.168.10.1
 standby 1 priority 120
 standby 1 preempt
```

**Office 2 (Router-on-a-Stick on R2):**

```bash
interface gi0/0.11
 encapsulation dot1Q 11
 ip address 192.168.11.1 255.255.255.0
```

### 5. Internal OSPF (Area 0 in both sites)

```bash
router ospf 1
 router-id X.X.X.X
 network 192.168.X.0 0.0.0.255 area 0
 network 10.10.10.X 0.0.0.3 area 0
```

### 6. DHCP Configuration

```bash
ip dhcp pool VLAN10-Pool
 network 192.168.10.0 255.255.255.0
 default-router 192.168.10.1
 dns-server 192.168.10.100
 domain-name ccna.local
```

### 7. HSRP on Distribution (Office 1)

```bash
interface vlan 10
 standby 1 ip 192.168.10.1
 standby 1 priority 120
 standby 1 preempt
```

### 8. GRE + BGP for Inter-Site Connectivity

```bash
interface Tunnel0
 ip address 10.0.0.1 255.255.255.252
 tunnel source Loopback0
 tunnel destination 4.4.4.4
!
router bgp 65001
 neighbor 10.0.0.2 remote-as 65002
 redistribute ospf 1
```

### 9. ISP-A and ISP-B Simulating Real Providers

- BGP between ISP-A and ISP-B (ASN 64500 - 64501)
- DHCP to simulate dynamic IP assignment for R1 and R2
- Default static routes + NAT + OSPF/BGP according to the scenario

### 10. Additional Services

```bash
snmp-server community OFFICE1 RO
logging 192.168.xx.100
logging trap debugging
```