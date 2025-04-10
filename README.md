# 🌐 Proyecto de Red Empresarial Multisede - CCNA LAB

> 🔧 Simulación completa de una red empresarial distribuida entre dos sedes, ideal para formación CCNA/SOC.

## 📘 Documentación

Este laboratorio representa una topología realista de red empresarial con sedes en **Barcelona** y **Madrid**, conectadas a través de una WAN simulada con **GRE** y **BGP** entre ISPs. Se implementan tecnologías de nivel empresarial:

- 🔁 OSPF para enrutamiento interno
- 👥 HSRP como gateway redundante
- 🌐 EtherChannel y Trunking
- 🔀 Router-on-a-Stick
- 🧪 DHCP, SSH, SNMP y logging
- 🧰 Preparado para escalar con NGFW, SIEM y herramientas de ciberseguridad

## 💼 Autor

**Nathan** — Ingeniero en Redes y Seguridad

---

## ✨ Objetivo

Simular un entorno profesional multisede con tecnologías clave de CCNA y fundamentos para SOC, diseñado para prácticas, troubleshooting y documentación técnica profesional.

---

## 🏢 Topología General

- 📍 [**Oficina 1 - Barcelona**](docs/es/logicaldesign.md#barcelona): Distribución con HSRP y enrutamiento distribuido
- 📍 [**Oficina 2 - Madrid**](docs/es/logicaldesign.md#madrid): Arquitectura Router-on-a-Stick
- 🌐 [**ISPs (ISP-A & ISP-B)**](): Simulan conectividad WAN mediante BGP con túneles GRE

📊 [Asignación de Direcciones IP](docs/es/ipassignments.md)  
✅ [Guía Paso a Paso de Configuración](docs/es/config.md)

---

## 📁 Archivos de Configuración `.ios`

Los archivos están disponibles por dispositivo para ser importados directamente en **Cisco Packet Tracer**.

🔹 Carpeta: [📁 `config/`](docs/)  
🔧 [Resolución de Incidencias y Troubleshooting](docs/es/troubleshooting.md)

---

# 🌍 Multi-Site Enterprise Network Project - CCNA LAB

> 💡 Full-scale enterprise network simulation for advanced CCNA/SOC practice

## 📘 Available Documentation

This lab represents a realistic corporate network topology with two sites (**Barcelona** and **Madrid**) connected via a simulated WAN using **GRE** and **BGP** between ISPs. It includes:

- 🔁 Internal routing with **OSPF**
- 👥 Redundant gateways using **HSRP**
- 🧱 Inter-VLAN routing with **Router-on-a-Stick**
- 🧪 DHCP, SSH, SNMP and system logging
- 🛡️ Scalable base for NGFW, SIEM, and cybersecurity tools

## 💼 Author

**Nathan** — Network & Security Engineer

---

## ✨ Objective

To simulate a professional-grade network infrastructure for hands-on CCNA and SOC-level training, with realistic services and documentation.

---

## 🏢 General Topology

- 🏢 [**Office 1 - Barcelona**](docs/en/logicaldesign.md#barcelona): Distributed routing with HSRP redundancy
- 🏢 [**Office 2 - Madrid**](docs/en/logicaldesign.md#madrid): Router-on-a-Stick core setup
- 🌐 [**ISPs (ISP-A & ISP-B)**](): Simulated WAN with GRE tunnels and BGP routing

📊 [IP Address Assignments](docs/en/ipassignments.md)  
✅ [Step-by-Step Configuration Guide](docs/en/config.md)

---

## 📁 Device-Specific `.ios` Files

All configurations are available by device for direct Packet Tracer import.

🔹 Folder: [📁 `config/`](docs/)  
🔧 [Troubleshooting and Issue Resolution](docs/en/troubleshooting.md)
