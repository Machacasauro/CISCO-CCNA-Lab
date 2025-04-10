# Proyecto de Red Empresarial Multisede - CCNA LAB

# 📘 Documentación disponible:

Este laboratorio representa una topología empresarial de dos sedes (Barcelona y Madrid), conectadas mediante una red WAN simulada con GRE y BGP entre ISPs, integrando OSPF para el enrutamiento interno, HSRP, EtherChannel, Router-on-a-Stick, DHCP, SSH, SNMP y logging.
## 💼 Autor

**Nathan** - Ingeniero en Redes y Seguridad

## ✨ Objetivo

Este entorno simula una red empresarial multisede realista orientada a formación en CCNA/SOC, preparada para escalar con NGFW, SIEM, y herramientas de ciberseguridad.


## 🏢 Topología General

- [**Oficina 1 - Barcelona**](docs/es/logicaldesign.md#barcelona): Distribución con HSRP y enrutamiento distribuido.
- [**Oficina 2 - Madrid**](docs/es/logicaldesign.md#madrid): Arquitectura Router-on-a-Stick.
- [**ISPs**]: ISP-A e ISP-B simulan conectividad WAN con BGP entre ellos.

- 📊 [Asignación de Direcciones IP](docs/es/ipassignments.md)
- ✅ [Paso a Paso de la Configuración](docs/es/config.md)


## 📁 Archivos .ios por dispositivo

Están disponibles individualmente para importarlos directamente en Packet Tracer. Consulta la carpeta 📁 [config/](docs/) del repositorio.

- 🔧 [Troubleshooting y Resolución de Incidencias](docs/es/troubleshooting.md)




# Multi-Site Enterprise Network Project - CCNA LAB

## 📘 Available documentation:

This lab represents a corporate network topology with two sites (Barcelona and Madrid), connected through a simulated WAN using GRE and BGP between ISPs. It integrates OSPF for internal routing, HSRP, EtherChannel, Router-on-a-Stick, DHCP, SSH, SNMP, and logging.

## 💼 Author

**Nathan** - Network & Security Engineer

## ✨ Objective

This environment simulates a realistic multi-site enterprise network designed for CCNA/SOC training, ready to scale with NGFW, SIEM, and cybersecurity tools.

## 🏢 General Topology

- [**Office 1 - Barcelona**](docs/en/logicaldesign.md#barcelona): Distribution with HSRP and distributed routing.
- [**Office 2 - Madrid**](docs/en/logicaldesign.md#madrid): Router-on-a-Stick architecture.
- [**ISPs**](): ISP-A and ISP-B simulate WAN connectivity with BGP peering.

- 📊 [IP Address Assignments](docs/en/ipassignments.md)
- ✅ [Step-by-Step Configuration Guide](docs/en/config.md)


## 📁 Device-Specific .ios Files

These are available individually for direct import into Packet Tracer. Check the 📁 [config/](docs/) folder in the repository.

- 🔧 [Troubleshooting and Issue Resolution](docs/en/troubleshooting.md)