
# 🛠️ Troubleshooting - CCNA Multisite Enterprise Network

This document describes the most common issues encountered during the lab deployment and the solutions applied.

---

## ⚡ Common Issues and Solutions

### 1. ❌ OSPF fails to establish neighbor relationship between R1 and R2 (via GRE)
**Symptom**: Logs such as:
```
%OSPF-5-ADJCHG: Neighbor Down: Dead timer expired
```
**Cause**:
- Tunnel0 interface was not advertised in OSPF.
- R1 was using a loopback as `tunnel source`, but it was not properly routable.

**Solution**:
```bash
router ospf 1
 network 10.0.0.0 0.0.0.3 area 0
```
Ensure the `tunnel destination` is reachable and appears in the routing table.

---

### 2. ⚠️ DHCP does not assign IP addresses
**Cause**:
- Misconfigured excluded-addresses.
- Missing `default-router` directive.

**Solution**:
Check `ip dhcp excluded-address` and confirm that the `default-router` matches the VLAN gateway.

---

### 3. ⚠️ STP blocks traffic between switches
**Cause**:
- Multiple trunk links without proper STP configuration.

**Solution**:
```bash
spanning-tree mode rapid-pvst
spanning-tree vlan X root primary
```
Leverage EtherChannel for redundant links and avoid loops.

---

### 4. ❌ BGP neighbor relationship not established
**Cause**:
- Incorrect IP for GRE interface.
- No IP reachability to remote peer.

**Solution**:
Verify:
```bash
ping 10.0.0.2
router bgp 65001
 neighbor 10.0.0.2 remote-as 65002
```

---

### 5. ❌ SSH fails to connect
**Cause**: Missing VTY configuration.
**Solution**:
```bash
line vty 0 4
 login local
 transport input ssh
```

---

### 6. ⚡ NAT interfering with GRE/BGP
**Cause**: NAT configured on tunnel interface.
**Solution**: Exclude GRE/BGP traffic from NAT (not required in PT but good practice in real-world environments).

---

## ✅ Final Recommendations
- Always test IP reachability before configuring routing protocols.
- Use `show ip route`, `show ip ospf neighbor`, and `show ip bgp summary` for troubleshooting.
- Always document and back up configurations per device.

---
