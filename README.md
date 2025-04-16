# 🌐 Proyecto de Red Empresarial Multisede - CCNA LAB

> 🔧 Simulación completa de una red empresarial distribuida entre dos sedes, ideal para formación CCNA/SOC.

## 📘 Documentación

Este laboratorio representa una topología realista de red empresarial con sedes en **Barcelona** y **Madrid**, conectadas a través de una WAN simulada con **GRE** y **BGP** entre ISPs. Se implementan tecnologías de nivel empresarial:

- 🔁 **OSPF** para enrutamiento interno
- 👥 **HSRP** como gateway redundante
- 🌐 **EtherChannel** y **Trunking**
- 🔀 **Router-on-a-Stick**
- 🧪 **DHCP**, **SSH**, **SNMP** y **logging**
- 🧰 Preparado para escalar con **NGFW**, **SIEM** y herramientas de **ciberseguridad**

## 💼 Autor

**Nathan CM** — Ingeniero en Redes y Seguridad

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
    [Resolución de Incidencias y Troubleshooting](docs/es/troubleshooting.md)
    [Archivo Packet Tracer V.8.2.2.0400](configs/CISCO-CCNA-Lab.pkt)
    [ACLs](configs/ACL_Policies.md)

---
# 🌐 Multi-Site Enterprise Network Project - CCNA LAB

> 🔧 Full simulation of a distributed enterprise network between two sites, ideal for CCNA/SOC training.

## 📘 Documentation

This lab represents a realistic enterprise network topology with offices in **Barcelona** and **Madrid**, connected through a simulated WAN using **GRE** and **BGP** between ISPs. Enterprise-grade technologies are implemented:

- 🔁 **OSPF** for internal routing  
- 👥 **HSRP** as a redundant gateway  
- 🌐 **EtherChannel** and **Trunking**  
- 🔀 **Router-on-a-Stick**  
- 🧪 **DHCP**, **SSH**, **SNMP**, and **logging**  
- 🧰 Ready to scale with **NGFW**, **SIEM**, and **cybersecurity tools**

## 💼 Author

**Nathan CM** — Network and Security Engineer

---

## ✨ Objective

To simulate a professional multi-site environment using core CCNA technologies and SOC fundamentals, designed for hands-on practice, troubleshooting, and professional technical documentation.

---

## 🏢 General Topology

- 📍 [**Office 1 - Barcelona**](docs/en/logicaldesign.md#barcelona): Distribution model with HSRP and distributed routing  
- 📍 [**Office 2 - Madrid**](docs/en/logicaldesign.md#madrid): Router-on-a-Stick architecture  
- 🌐 [**ISPs (ISP-A & ISP-B)**](): Simulate WAN connectivity via BGP and GRE tunnels  

📊 [IP Address Assignment](docs/en/ipassignments.md)  
✅ [Step-by-Step Configuration Guide](docs/en/config.md)

---

## 📁 `.ios` Configuration Files

The configuration files are available per device for direct import into **Cisco Packet Tracer**.

🔹 Folder: [📁 `config/`](docs/)  
🔧 [Troubleshooting and Issue Resolution](docs/en/troubleshooting.md)

---