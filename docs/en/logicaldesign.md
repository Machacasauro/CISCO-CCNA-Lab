# 🧭 Logical Network Design

This document describes the logical architecture of both offices in the project: Office 1 (Barcelona) and Office 2 (Madrid).

---

## 🏢 Office 1 
## Barcelona

The network is designed with high availability, logical segmentation using VLANs, and redundancy at both Layer 2 and Layer 3.

### 📐 Logical Components

#### 1. **Access Switches (ASW-A1, ASW-A2, ASW-A3)**
- Connect end devices to the network.
- Functional VLANs: 10 (Net), 20 (Servers), 30 (Guests), 99 (Management).
- Redundant trunk connections to distribution switches using L2 EtherChannel.

#### 2. **Distribution Switches (DSW-A1, DSW-A2)**
- Perform Inter-VLAN routing through SVIs.
- Configured with HSRP for virtual gateways.
- L3 communication between them via EtherChannel.
- Participate in OSPF (area 0) along with Router R1.

#### 3. **Router R1**
- Connected to both DSWs via point-to-point L3 links.
- Announces the default route using `default-information originate`.
- DHCP server for all VLANs.
- Supports GRE tunnels for inter-site connectivity.
- Participates in OSPF area 0 and BGP for WAN routing.

### 🔁 Redundancy and Security

- **HSRP** in all VLANs (virtual IPs).
- **Rapid-PVST+** as the STP protocol.
- **DHCP relay** via `ip helper-address` on SVIs.
- EtherChannel for link aggregation.

### 🕸️ Logical Diagram

```
[PCs]--ASW--+
            |
        [DSW-A1]====[DSW-A2]
            |         |
         Gi0/0     Gi0/1
            \     //
              [ R1 ]
```

---

## 🏢 Office 2 
## Madrid

The network uses a Router-on-a-Stick architecture with subinterfaces on R2 for InterVLAN routing.

### 📐 Logical Components

#### 1. **Access Switches (ASW-B1, ASW-B2, ASW-B3)**
- Connect end devices to VLANs 11 (Net), 21 (Servers), 31 (Guests), 99 (Management).
- Trunk to the distribution switch (DSW-B1).
- Participate in L2 EtherChannel to DSW-B1/B2.

#### 2. **Distribution Switches (DSW-B1, DSW-B2)**
- Trunk links to ASWs.
- Trunk to Router R2.
- Participate in Rapid-PVST+ STP.
- Do not perform InterVLAN routing.

#### 3. **Router R2**
- Router-on-a-Stick using `Gi0/0.X` subinterfaces for each VLAN.
- DHCP server for VLANs 11, 21, 31, 99, and 100.
- Participates in OSPF (area 0) and BGP (AS 65002).
- GRE tunnel to R1 (Office 1) via ISP-B.
- Default route to ISP-B.

### 🔁 Security and Control

- `spanning-tree portfast` + `bpduguard` on access ports.
- Rapid-PVST+ STP with adjusted root bridge.
- DHCP excluded addresses for infrastructure.
- Logging and SNMP configured.

### 🕸️ Logical Diagram

```
[PCs]--ASW--+
            |
        [DSW-B1]====[DSW-B2]
             |
            [R2]---GRE---[R1]
```

---