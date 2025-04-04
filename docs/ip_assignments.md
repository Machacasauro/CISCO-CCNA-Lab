
# 📊 Asignación de Direcciones IP

Este archivo documenta el esquema de direccionamiento IP utilizado en la red OpenSEC.

## 🌐 Enlaces Punto a Punto (WAN/L3)

| Dispositivo | Interfaz       | IP               | Descripción                       |
|-------------|----------------|------------------|-----------------------------------|
| R1          | Gi0/0          | 10.10.10.254/30  | Hacia DSW-A1                      |
| DSW-A1      | Gi1/0/1        | 10.10.10.253/30  | Hacia R1                          |
| R1          | Gi0/1          | 10.10.10.250/30  | Hacia DSW-A2                      |
| DSW-A2      | Gi1/0/1        | 10.10.10.249/30  | Hacia R1                          |
| DSW-A1      | Port-Channel2  | 10.10.10.1/30    | L3 hacia DSW-A2                   |
| DSW-A2      | Port-Channel2  | 10.10.10.2/30    | L3 hacia DSW-A1                   |

## 🖧 VLANs

| VLAN | Nombre        | Red/Subred          | Gateway HSRP IP | IP DSW-A1       | IP DSW-A2       |
|------|---------------|---------------------|------------------|------------------|------------------|
| 10   | Usuarios       | 192.168.10.0/24     | 192.168.10.1     | 192.168.10.2     | 192.168.10.3     |
| 20   | Servidores     | 192.168.20.0/24     | 192.168.20.1     | 192.168.20.2     | 192.168.20.3     |
| 30   | Guest          | 192.168.30.0/24     | 192.168.30.1     | 192.168.30.2     | 192.168.30.3     |
| 99   | MGMT           | 192.168.99.0/24     | 192.168.99.1     | 192.168.99.2     | 192.168.99.3     |

## 🧞 DHCP (R1)

- El router R1 entrega direcciones IP dinámicas a todas las VLANs.
- El helper-address configurado en los SVIs asegura el relay del tráfico DHCP hacia R1.
