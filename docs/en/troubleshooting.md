
# 🛠️ Troubleshooting - CCNA Multi-site Enterprise Network

This document describes the issues encountered during the development of the lab and the solutions applied.



## ⚡ Common Issues and Solutions

### 1. ❌ OSPF does not establish neighbors between R1 and R2 (via GRE)
**Symptom**: Messages like:
```
%OSPF-5-ADJCHG: Neighbor Down: Dead timer expired
```
**Cause**:
- Tunnel0 was not advertised in OSPF.
- R1 was using loopback as `tunnel source` but it was not properly routable.

**Solution**:
```bash
router ospf 1
 network 10.0.0.0 0.0.0.3 area 0
```
Ensure the `tunnel destination` is reachable and appears in the routing table.



### 2. ⚠️ DHCP not assigning IP addresses
**Cause**:
- Incorrectly defined excluded-addresses.
- No default-router configured.

**Solution**:
Check the `ip dhcp excluded-address` and ensure the `default-router` belongs to the corresponding VLAN.



### 3. ⚠️ STP blocking traffic between switches
**Cause**:
- Multiple uncontrolled links.

**Solution**:
```bash
spanning-tree mode rapid-pvst
spanning-tree vlan X root primary
```
Use EtherChannel properly and avoid loops.



### 4. ❌ BGP does not establish neighbor relationships
**Cause**:
- Incorrect GRE IP configuration
- No connectivity to the destination IP

**Solution**:
Verify:
```bash
ping 10.0.0.2
router bgp 65002
 neighbor 10.0.0.2 remote-as 65001
```



### 5. ❌ SSH not working
**Cause**: Missing commands under `line vty`
**Solution**:
```bash
line vty 0 4
 login local
 transport input ssh
```



### 6. ⚡ NAT blocking GRE/BGP
**Cause**: NAT applied to the tunnel interface
**Solution**: Exclude GRE/BGP traffic from NAT (not required in Packet Tracer, but best practice in real environments)



## ✅ Final Recommendations
- Always verify IP connectivity before configuring routing protocols.
- Use `show ip route`, `show ip ospf neighbor`, `show ip bgp summary` for diagnostics.
- Always document your changes and maintain per-device backups.


