# 📊 IP Address Assignment

This file documents the IP addressing scheme used in the OpenSEC network, which connects two locations: Office 1 (Barcelona) and Office 2 (Madrid), through a GRE tunnel over two ISPs.

---

## 🌐 Point-to-Point Links (WAN and GRE)

| Device      | Interface      | IP Address       | Description                         |
|-------------|----------------|------------------|-------------------------------------|
| R1          | Gi0/0          | 10.10.10.254/30  | To DSW-A1                           |
| R1          | Gi0/1          | 10.10.10.250/30  | To DSW-A2                           |
| R1          | Gi0/0/0        | 200.1.1.2/30     | To ISP-A                            |
| R1          | Loopback0      | 1.1.1.1/32       | GRE Tunnel Source                   |
| R1          | Tunnel0        | 10.0.0.1/30      | GRE Tunnel to R2                    |
| DSW-A1      | Gi1/0/1        | 10.10.10.253/30  | To R1                               |
| DSW-A2      | Gi1/0/1        | 10.10.10.249/30  | To R1                               |
| ISP-A       | Gi0/0          | 200.1.1.1/30     | To R1                               |
| ISP-B       | Gi0/0          | 200.3.3.1/30     | To R2                               |
| R2          | Gi0/0/0        | 200.3.3.2/30     | To ISP-B                            |
| R2          | Tunnel0        | 10.0.0.2/30      | GRE Tunnel to R1                    |
| R2          | Loopback0      | 4.4.4.4/32       | GRE Tunnel Source                   |

---

## 🧞 VLANs Office 1 (Barcelona)

| VLAN | Name       | Subnet              | HSRP Gateway     | DSW-A1 IP        | DSW-A2 IP        |
|------|------------|---------------------|------------------|------------------|------------------|
| 10   | NET        | 192.168.10.0/24     | 192.168.10.1     | 192.168.10.2     | 192.168.10.3     |
| 20   | SERVERS    | 192.168.20.0/24     | 192.168.20.1     | 192.168.20.2     | 192.168.20.3     |
| 30   | GUEST      | 192.168.30.0/24     | 192.168.30.1     | 192.168.30.2     | 192.168.30.3     |
| 99   | MGMT       | 192.168.99.0/24     | 192.168.99.1     | 192.168.99.2     | 192.168.99.3     |

---

## 🧞 VLANs Office 2 (Madrid)

| VLAN | Name       | Subnet              | Gateway          |
|------|------------|---------------------|------------------|
| 11   | NET        | 192.168.11.0/24     | 192.168.11.1     |
| 21   | SERVERS    | 192.168.21.0/24     | 192.168.21.1     |
| 31   | GUEST      | 192.168.31.0/24     | 192.168.31.1     |
| 100  | MGMT       | 192.168.100.0/24    | 192.168.100.1    |

---

## 🎯 DHCP

- 🖥️ **Office 1**: DHCP active on R1 for VLANs 10, 20, 30, and 99.
- 🖥️ **Office 2**: DHCP active on R2 for VLANs 11, 21, 31, and 100.
- 📢 `ip helper-address` is used on access switches to forward DHCP requests ⚠️ Only on devices performing Inter-VLAN routing.

---

## 🛰️ GRE Tunnel

| Device | Interface | Tunnel IP     | Notes                                |
|--------|-----------|---------------|--------------------------------------|
| R1     | Tunnel0   | 10.0.0.1/30   | Uses Loopback0 as source             |
| R2     | Tunnel0   | 10.0.0.2/30   | Uses Loopback0 as source             |

---
