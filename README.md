# Proyecto de Red Empresarial Multisede - CCNA LAB

Este laboratorio representa una topología empresarial de dos sedes (Barcelona y Madrid), conectadas mediante una red WAN simulada con GRE y BGP entre ISPs, integrando OSPF para el enrutamiento interno, HSRP, EtherChannel, Router-on-a-Stick, DHCP, SSH, SNMP y logging.
## 💼 Autor

**Nathan** - Especialista en Redes y Seguridad

---

## ✨ Objetivo

Este entorno simula una red empresarial multisede realista orientada a formación en CCNA/SOC, preparada para escalar con NGFW, SIEM, y herramientas de ciberseguridad.

---

## 🏢 Topología General

- **Oficina 1 - Barcelona**: Distribución con HSRP y enrutamiento distribuido.
- **Oficina 2 - Madrid**: Arquitectura Router-on-a-Stick.
- **ISPs**: ISP-A e ISP-B simulan conectividad WAN con BGP entre ellos.

---

## ✅ Paso a Paso de la Configuración

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

---

### 2. VTP y VLANs

**Oficina 1 - Barcelona**:

```bash
vtp mode server
vtp domain of1
vtp password of1
vlan 10,20,30,99
```

**Oficina 2 - Madrid**:

```bash
vtp mode server
vtp domain of2
vtp password of2
vlan 11,21,31,99
```

---

### 3. EtherChannel

**L2 EtherChannel en DSW ↔ ASW**:

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
 ip address x.x.x.x y.y.y.y
```

---

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

---

### 5. OSPF Interno (Area 0 en todas las sedes)

```bash
router ospf 1
 router-id X.X.X.X
 network 192.168.X.0 0.0.0.255 area 0
 network 10.10.10.X 0.0.0.3 area 0
```

---

### 6. DHCP

```bash
ip dhcp pool VLAN10-Pool
 network 192.168.10.0 255.255.255.0
 default-router 192.168.10.1
 dns-server 192.168.10.100
 domain-name ccna.local
```

---

### 7. HSRP en Distribución (Oficina 1)

```bash
interface vlan 10
 standby 1 ip 192.168.10.1
 standby 1 priority 120
 standby 1 preempt
```

---

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

---

### 9. ISP-A y ISP-B simulando proveedores reales

- BGP entre ISP-A y ISP-B (ASN 64500 - 64501)
- DHCP para simular asignación automática de IP en R1 y R2
- Rutas estáticas por defecto + NAT + OSPF/BGP según simulación

---

### 10. Servicios adicionales

```bash
snmp-server community OFICINA1 RO
logging 192.168.20.100
logging trap debugging
```

---

## 📁 Archivos .ios por dispositivo

Están disponibles individualmente para importarlos directamente en Packet Tracer. Consulta la carpeta `configs/` del repositorio.

---



