
# 🧭 Diseño Lógico de la Red Oficina 1 

La red está diseñada siguiendo un modelo de red empresarial escalable con redundancia, seguridad y segmentación.

## 📐 Componentes Lógicos

### 1. **Switches de Acceso (ASW-A1, A2, A3)**
- Conectan dispositivos finales a la red.
- VLANs definidas por función (10, 20, 30, 99).
- Conectados en trunk redundante hacia los switches de distribución mediante EtherChannel L2.

### 2. **Switches de Distribución (DSW-A1, DSW-A2)**
- Ejecutan enrutamiento InterVLAN (SVIs).
- Proveen alta disponibilidad con HSRP.
- Enlace L3 entre ambos para resiliencia.
- Participan en OSPF para enrutamiento dinámico hacia R1.

### 3. **Router R1**
- Conectado vía L3 a ambos DSW.
- Anuncia la default route vía OSPF.
- Ofrece servicio DHCP centralizado.
- Puede actuar como salida a Internet o túnel GRE.

## 🔁 Redundancia y Seguridad

- HSRP en VLANs 10, 20, 30 y 99 con IPs virtuales
- STP (Rapid-PVST+) para evitar loops en capa 2
- DHCP relay (`ip helper-address`) en SVIs para permitir distribución de IP desde R1

## 🕸️ Diagrama Lógico (estructura)

```
[PCs]--ASW--+
            |
        [DSW-A1]====[DSW-A2]
            |         |
           R1 (core router con DHCP)
```
