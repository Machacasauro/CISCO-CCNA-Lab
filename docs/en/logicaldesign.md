# 🧭 Network Logical Design

This document describes the logical architecture of both project sites: Office 1 (Barcelona) and Office 2 (Madrid).



## 🏢 Office 1
## Barcelona

The network is designed with high availability, logical segmentation through VLANs, and redundancy at both Layer 2 and Layer 3.

### 📐 Logical Components

#### 1. **Access Switches (ASW-A1, ASW-A2, ASW-A3)**
- Connect end-user devices to the network.
- Functional VLANs: 10 (Network), 20 (Servers), 30 (Guests), 40 (Voice) and 99 (Management).
- Redundant trunk connections to distribution switches via Layer 2 EtherChannel.

#### 2. **Distribution Switches (DSW-A1, DSW-A2)**
- Perform Inter-VLAN routing via SVIs.
- Configured with HSRP for virtual gateway addresses.
- Participates in OSPF (area 0) and BGP (AS 65002).
- Layer 3 communication between them via EtherChannel.

#### 3. **Router R1**
- Connected to both DSWs via point-to-point Layer 3 links.
- Advertises the default route using `default-information originate`.
- DHCP server for all VLANs.
- Supports GRE tunnels for inter-site connectivity.
- Participates in OSPF area 0 and BGP for WAN exchange.

### 🔁 Redundancy and Security

- **HSRP** on all VLANs (virtual IPs).
- **Rapid-PVST+** as the STP protocol.
- **DHCP relay** via `ip helper-address` on SVIs.
- EtherChannel used for link aggregation.

### 🕸️ Logical Diagram

```
[PCs]--ASW(1/2/3)
            |           |
        [DSW-A1]====[DSW-A2]
            |           |
         [Gi0/0]     Gi0/1
            \\       //
               [R1]  NAT -> [ISP]  [R2]
                     GRE 
```



## 🏢 Office 2
## Madrid

The network uses a Router-on-a-Stick architecture with subinterfaces on R2 for Inter-VLAN routing.

### 📐 Logical Components

#### 1. **Access Switches (ASW-B1, ASW-B2, ASW-B3)**
- Connect end devices to VLANs 11 (Network), 21 (Servers), 31 (Guests), 99 (Management).
- Trunks connected to the distribution switch (DSW-B1).
- Participate in Layer 2 EtherChannel to DSW-B1/B2.

#### 2. **Distribution Switches (DSW-B1, DSW-B2)**
- Trunk links to access switches.
- Trunk connection to router R2.
- Participate in Rapid-PVST+ STP.
- Do not perform Inter-VLAN routing.

#### 3. **Router R2**
- Router-on-a-Stick with subinterfaces `Gi0/0.X` for each VLAN.
- DHCP server for VLANs 11, 21, 31, 99, and 100.
- Participates in OSPF (area 0) and BGP (AS 65001).
- GRE tunnel to R1 (Office 1) via ISP-B.
- Default route pointing to ISP-B.
- It does not perform NAT between VLANs, only for traffic destined to the Internet (WAN)

### 🔁 Security and Control

- `spanning-tree portfast` + `bpduguard` on access ports.
- Rapid-PVST+ STP with root bridge tuning.
- DHCP excluded-address ranges for infrastructure.
- Logging and SNMP configured.

### 🕸️ Logical Diagram

```
[PCs]--ASW(1/2/3)--+-
            |           |
        [DSW-B1]====[DSW-B2]
             |
            [R2] NAT -> [ISP]  [R1]
                 GRE -
```
