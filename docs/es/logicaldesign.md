# 🧭 Diseño Lógico de la Red

Este documento describe la arquitectura lógica de ambas sedes del proyecto: Oficina 1 (Barcelona) y Oficina 2 (Madrid).

---

## 🏢 Oficina 1 
## Barcelona

La red está diseñada con alta disponibilidad, segmentación lógica mediante VLANs, y redundancia en capa 2 y capa 3.

### 📐 Componentes Lógicos

#### 1. **Switches de Acceso (ASW-A1, ASW-A2, ASW-A3)**
- Conectan dispositivos finales a la red.
- VLANs funcionales: 10 (Red), 20 (Servidores), 30 (Invitados), 40 (Voz) y 99 (Administración).
- Conexiones trunk redundantes a los switches de distribución mediante EtherChannel L2.

#### 2. **Switches de Distribución (DSW-A1, DSW-A2)**
- Ejecutan enrutamiento Inter-VLAN mediante SVIs.
- Configurados con HSRP para puertas de enlace virtuales.
- Participa en OSPF (área 0).
- Comunicación L3 entre ellos mediante EtherChannel.


#### 3. **Router R1**
- Conectado a ambos DSW mediante enlaces punto a punto L3.
- Anuncia la ruta por defecto con `default-information originate`.
- Servidor DHCP para todas las VLANs.
- Soporta túneles GRE para conectividad inter-sede.
- Participa en OSPF área 0 y BGP para intercambio WAN.

### 🔁 Redundancia y Seguridad

- **HSRP** en todas las VLANs (IPs virtuales).
- **Rapid-PVST+** como protocolo STP.
- **DHCP relay** mediante `ip helper-address` en los SVIs.
- EtherChannel para agregación de enlaces.

### 🕸️ Diagrama Lógico

```
[PCs]--ASW(1/2/3)---------
            |           |
        [DSW-A1]====[DSW-A2]
            |           |
         [Gi0/0]     Gi0/1
            \\      //
               [R1] --- NAT ----> [ISP] --- [R2]
                    --- GRE ---
```

---

## 🏢 Oficina 2 
## Madrid

La red utiliza una arquitectura Router-on-a-Stick con subinterfaces en R2 para el enrutamiento InterVLAN.

### 📐 Componentes Lógicos

#### 1. **Switches de Acceso (ASW-B1, ASW-B2, ASW-B3)**
- Conectan dispositivos a las VLANs 10 (Red), 20 (Servidores), 30 (Invitados), 40 (voz) y 99 (Administración).
- Trunk hacia el switch de distribución (DSW-B1).
- Participan en EtherChannel L2 hacia DSW-B1/B2.

#### 2. **Switches de Distribución (DSW-B1, DSW-B2)**
- Enlaces trunk hacia ASW.
- Trunk hacia router R2.
- Participan en STP Rapid-PVST+.
- No hacen enrutamiento InterVLAN.

#### 3. **Router R2**
- Router-on-a-Stick con subinterfaces `Gi0/0.X` para cada VLAN.
- Servidor DHCP para VLANs 10, 20, 30, 40 y 99.
- Participa en OSPF (área 0) y BGP (AS 65001).
- Túnel GRE hacia R1 (Oficina 1) por medio de ISP-B.
- Default route hacia ISP-B.
- No hace NAT entre las VLANs solo al tráfico a Internet (WAN)

### 🔁 Seguridad y Control

- `spanning-tree portfast` + `bpduguard` en puertos de acceso.
- STP Rapid-PVST+ con root bridge ajustado.
- IPs excluidas del DHCP para infraestructura.
- Logging y SNMP configurados.

### 🕸️ Diagrama Lógico

```
[PCs]--ASW(1/2/3)--+-------------
            |           |
        [DSW-B1]====[DSW-B2]
             |
            [R2]--- NAT ----> [ISP] --- [R1]
                --- GRE ----
```
---