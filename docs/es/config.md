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
interface range gi1/0/2 - 4
 channel-group 1 mode desirable
interface port-channel 1
 switchport mode trunk
``` 

**L3 EtherChannel entre DSW-A1 ↔ DSW-A2:**

```bash
interface range gi1/0/5 - 6
 no switchport
 channel-group 2 mode desirable
interface port-channel 2
 no switchport
 ip address 10.10.10.1 255.255.255.252
``` 

### 4. Enrutamiento Inter-VLAN

**Oficina 1 (Distribución con SVIs):**

```bash
interface vlan 10
 ip address 192.168.10.2 255.255.255.0
 standby 1 ip 192.168.10.1
 standby 1 priority 120
 standby 1 preempt
``` 

**Oficina 2 (Router-on-a-Stick en R2):**

```bash
interface gi0/0.11
 encapsulation dot1Q 11
 ip address 192.168.11.1 255.255.255.0
``` 

### 5. OSPF Interno (Área 0 en todas las sedes)

```bash
router ospf 1
 router-id 1.1.1.1
 network 192.168.10.0 0.0.0.255 area 0
 network 192.168.20.0 0.0.0.255 area 0
 network 192.168.30.0 0.0.0.255 area 0
 network 192.168.99.0 0.0.0.255 area 0
 network 10.10.10.0 0.0.0.255 area 0
``` 

### 6. DHCP

```bash
ip dhcp excluded-address 192.168.10.1 192.168.10.10
ip dhcp pool VLAN10-Pool
 network 192.168.10.0 255.255.255.0
 default-router 192.168.10.1
 dns-server 192.168.10.100
 domain-name ccna.local
``` 

### 7. HSRP en Distribución (Oficina 1)

```bash
interface vlan 10
 standby version 2
 standby 1 ip 192.168.10.1
 standby 1 priority 120
 standby 1 preempt
``` 

### 8. GRE + BGP para interconexión entre sedes

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

### 9. ISP-A y ISP-B simulando proveedores reales

- BGP entre ISP-A y ISP-B (ASN 64500 - 64501)
- DHCP para simular asignación automática de IP en R1 y R2
- Rutas estáticas por defecto + NAT + OSPF/BGP según simulación

### 10. Servicios adicionales

```bash
snmp-server community OFICINA1 RO
logging 192.168.20.100
logging trap debugging
```

