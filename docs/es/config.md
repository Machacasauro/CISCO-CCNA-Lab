### 1. Configuración básica en todos los dispositivos

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


### 2. VTP y VLANs

**Oficina 1 - Barcelona**:

```bash
vtp version 2
vtp mode server
vtp domain ccna
vtp password ccna
vlan 10,20,30,40,99
``` 

**Oficina 2 - Madrid**:

```bash
vtp version 2
vtp mode server
vtp domain ccna
vtp password ccna
vlan 10,20,30,40,99
``` 

> ⚠️ Atención: Recordar poner los demás equipos de tu red como cliente para que se pueble la base de datos VLAN:

```bash
vtp version 2
vtp mode client
vtp domain ccna
vtp password ccna
``` 

### 3. EtherChannel

**L2 EtherChannel entre DSW ↔ ASW**:

```bash
# DSW-Ax
interface range gi1/0/2 - 4
 switchport trunk native vlan 1000
 switchport trunk allowed vlan 10,20,30,40,99
 switchport mode trunk
 switchport nonegotiate
!
# ASW-Ax
interface range gi0/1 - 2
 switchport trunk native vlan 1000
 switchport trunk allowed vlan 10,20,30,40,99
 switchport mode trunk
 switchport nonegotiate
!

**L3 EtherChannel entre DSW-A1 ↔ DSW-A2:**

```bash
interface GigabitEthernet1/0/5
 no switchport
 no ip address
 channel-group 1 mode desirable
 ip ospf 1 area 0
 duplex auto
 speed auto
!
interface GigabitEthernet1/0/6
 no switchport
 no ip address
 channel-group 1 mode desirable
 ip ospf 1 area 0
 duplex auto
 speed auto
!
interface Port-channel1
 no switchport
 ip address 10.0.0.1 255.255.255.252
!

### 4. Enrutamiento Inter-VLAN

**Oficina 1 (Distribución con SVIs):**

```bash
interface vlan 10
 ip address 192.168.1.2 255.255.255.0
```

**Oficina 2 (Router-on-a-Stick en R2):**

```bash
interface gi0/0.10
 encapsulation dot1Q 10
 ip address 192.168.17.0 255.255.255.0
```

### 5. OSPF Interno (Área 0 en todas las sedes)

```bash
router ospf 1
 router-id 5.5.5.5
 log-adjacency-changes
 passive-interface Loopback0
 passive-interface Vlan10
 passive-interface Vlan20
 passive-interface Vlan30
 passive-interface Vlan40
 network 5.5.5.5 0.0.0.0 area 0
 network 192.168.1.0 0.0.0.255 area 0
 network 192.168.2.0 0.0.0.255 area 0
 network 192.168.3.0 0.0.0.255 area 0
 network 192.168.4.0 0.0.0.255 area 0
 network 192.168.99.0 0.0.0.15 area 0
 network 10.0.0.0 0.0.0.3 area 0
```

### 6. DHCP

```bash
ip dhcp excluded-address 192.168.1.1 192.168.1.10
ip dhcp pool VLAN10-Pool
 network 192.168.1.0 255.255.255.0
 default-router 192.168.1
 dns-server 192.168.2.100
 domain-name ccna.local
```

### 7. HSRP en Distribución (Oficina 1)

```bash
interface Vlan10
 ip address 192.168.1.2 255.255.255.0
 ip helper-address 10.10.10.254
 ip access-group NET-INTERNET-SOME-IN in
 standby 10 ip 192.168.1.1
 standby 10 priority 120
 standby 10 preempt
```

### 8. GRE + BGP para interconexión entre sedes

```bash
interface Tunnel0
 ip address 10.255.255.1 255.255.255.252
 mtu 1476
 tunnel source GigabitEthernet0/0/0
 tunnel destination 200.3.3.2
!
router bgp 65002
 bgp log-neighbor-changes
 no synchronization
 neighbor 200.1.1.1 remote-as 65010 (ISP-A)
```


### 9. ISP-A y ISP-B simulando proveedores reales (NAT)

- BGP entre ISP-A y ISP-B (ASN 65010- 65020)
- Asiganción estática de IP en R1 y R2
- Rutas estáticas por defecto + NAT + OSPF/BGP según simulación

```bash
ip nat inside source list 100 interface GigabitEthernet0/0/0 overload
```
```bash
access-list 100 deny ip 192.168.0.0 0.0.7.255 192.168.16.0 0.0.7.255
access-list 100 deny ip 192.168.99.0 0.0.0.15 192.168.99.16 0.0.0.15
access-list 100 permit ip 192.168.1.0 0.0.0.255 any
access-list 100 permit ip 192.168.2.0 0.0.0.255 any
access-list 100 permit ip 192.168.3.0 0.0.0.255 any
access-list 100 permit ip 192.168.4.0 0.0.0.255 any
access-list 100 permit ip 192.168.99.0 0.0.0.15 any
!
```
### 10. Servicios adicionales SNMP y logging

```bash
snmp-server community OFICINA1 RO
logging 192.168.2.100
logging trap debugging
```