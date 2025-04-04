# OpenSEC Network (Modelo Distribuido con OSPF, HSRP y DHCP Centralizado)

Este proyecto describe una infraestructura de red empresarial simulada en Cisco Packet Tracer, basada en enrutamiento distribuido con switches de capa 3, redundancia HSRP, OSPF para failover y un router central con DHCP.

## 🧱 Topología General

- **R1**: Router principal, DHCP server, default gateway hacia Internet/GRE
- **DSW-A1 & DSW-A2**: Switches de distribución (Capa 3) con SVIs, HSRP y OSPF
- **ASW-A1/A2/A3**: Switches de acceso con trunks y Port-Channels hacia los DSW

## 🧠 Características Técnicas

- InterVLAN Routing por SVIs en DSW-A1/A2
- Redundancia de gateway con HSRP (.1 virtual, .2 y .3 físicas)
- OSPF para propagación de rutas y default route desde R1
- DHCP centralizado en R1
- STP (Rapid-PVST+) para evitar loops en capa 2
- EtherChannel L2 entre ASW y DSW, L3 entre DSWs

## 📦 Objetivo

Sirve como plantilla base para:
- Montar laboratorios CCNA/CCNP
- Simular redes empresariales
- Mostrar experiencia profesional en GitHub/Portafolio
