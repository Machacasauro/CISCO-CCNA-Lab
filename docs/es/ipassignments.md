# IP Address Assignment

Este archivo documenta el esquema de direccionamiento IP utilizado en la red empresarial CISCO CCNA LAB, que conecta dos ubicaciones: Office 1 (Barcelona) y Office 2 (Madrid), a través de un túnel GRE a través de dos ISP.



## Point-to-Point Links (WAN and GRE)

| Device      | Interface      | IP Address       | Description                         |
|-|-||-|
| R1          | Gi0/0          | 10.10.10.254/30  | To DSW-A1                           |
| R1          | Gi0/1          | 10.10.10.250/30  | To DSW-A2                           |
| R1          | Gi0/0/0        | 200.1.1.2/30     | To ISP-A                            |
| R1          | Loopback0      | 1.1.1.1/32       | GRE Tunnel Source                   |
| R1          | Tunnel0        | 10.0.0.1/30      | GRE Tunnel to R2                    |
| DSW-A1      | Gi1/0/1        | 10.10.10.253/30  | To R1                               |
| DSW-A2      | Gi1/0/1        | 10.10.10.249/30  | To R1                               |
| DSW-A1      | Loopback0      | 5.5.5.5/32       | Routing Identifier                  |
| DSW-A2      | Loopback0      | 6.6.6.6/32       | Routing Identifier                  |
| ISP-A       | Gi0/0          | 200.1.1.1/30     | To R1                               |
| ISP-B       | Gi0/0          | 200.3.3.1/30     | To R2                               |
| R2          | Gi0/0/0        | 200.3.3.2/30     | To ISP-B                            |
| R2          | Tunnel0        | 10.0.0.2/30      | GRE Tunnel to R1                    |
| R2          | Loopback0      | 2.2.2.2/32       | GRE Tunnel Source                   |




## VLANs - Office 1 (Barcelona)

| VLAN | Name       | Subnet              | HSRP Gateway     | DSW-A1 IP        | DSW-A2 IP        |
|||||||
| 10   | NET        | 192.168.1.0/24      | 192.168.1.1      | 192.168.1.2      | 192.168.1.3      |
| 20   | SERVERS    | 192.168.2.0/24      | 192.168.2.1      | 192.168.2.2      | 192.168.2.3      |
| 30   | GUEST      | 192.168.3.0/24      | 192.168.3.1      | 192.168.3.2      | 192.168.3.3      |
| 40   | VOICE      | 192.168.4.0/24      | 192.168.4.1      | 192.168.4.2      | 192.168.4.3      |
| 99   | MGMT       | 192.168.99.0/28     | 192.168.99.1     | 192.168.99.2     | 192.168.99.3     |



## VLANs - Office 2 (Madrid)

| VLAN | Name       | Subnet              | Gateway          |
|||||
| 10   | NET        | 192.168.17.0/24     | 192.168.17.1     |
| 20   | SERVERS    | 192.168.18.0/24     | 192.168.18.1     |
| 30   | GUEST      | 192.168.19.0/24     | 192.168.19.1     |
| 40   | VOICE      | 192.168.20.0/24     | 192.168.20.1     |
| 99   | MGMT       | 192.168.99.16/28    | 200.3.3.2        |



## DHCP Overview

- **Office 1**: DHCP active on R1 for VLANs 10, 20, 30, and 99.
- **Office 2**: DHCP active on R2 for VLANs 10, 20, 30, and 99.
- `ip helper-address` se utiliza en los switches de acceso para reenviar solicitudes DHCP. ⚠️ Aplicable solo en dispositivos que realizan enrutamiento entre VLAN.



## GRE Tunnel

| Device | Interface | Tunnel IP     | Notes                                |
|--|--||--|
| R1     | Tunnel0   | 10.0.0.1/30   | Uses Loopback0 (1.1.1.1) as source   |
| R2     | Tunnel0   | 10.0.0.2/30   | Uses Loopback0 (4.4.4.4) as source   |

