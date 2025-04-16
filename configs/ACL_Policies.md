
# 🔐 ACL Policies - Barcelona & Madrid

This document provides a structured and readable representation of Access Control Lists (ACLs) configured per VLAN interface across both office sites in the enterprise network.

---

## 📍 Barcelona

```bash
interface vlan 10
 ip access-group NET-INTERNET-SOME-IN in
 ip access-list extended NET-INTERNET-SOME-IN

 remark  --- PERMIT MULTICAST ---
 permit ip 192.168.17.0 0.0.0.255 224.0.0.0 0.0.0.255
 remark --- PERMIT DHCP ---
 permit udp 192.168.17.0 0.0.0.255 any eq 67
 permit udp any eq 68 192.168.17.0 0.0.0.255 eq 67

 remark --- PERMIT DNS TO INTERNAL SERVER ---
 permit udp 192.168.17.0 0.0.0.255 host 192.168.2.100 eq 53
 permit tcp 192.168.17.0 0.0.0.255 host 192.168.2.100 eq 53

 remark --- PERMIT INTER-VLAN 10 ---
 permit ip 192.168.17.0 0.0.0.255 192.168.17.0 0.0.0.255

 remark --- DENY ACCESS TO INTERNAL RFC1918 ---
 deny ip 192.168.17.0 0.0.0.255 192.168.2.0 0.0.0.255
 deny ip 192.168.17.0 0.0.0.255 192.168.3.0 0.0.0.255
 deny ip 192.168.17.0 0.0.0.255 192.168.4.0 0.0.0.255
 deny ip 192.168.17.0 0.0.0.255 192.168.99.0 0.0.0.255
 deny ip 192.168.17.0 0.0.0.255 192.168.16.0 0.0.7.255

 remark --- PERMIT ANY ---
 permit ip any any

... (otros bloques de interfaces vlan 20, 30, 40, 99 de Barcelona)
```

---

## 📍 Madrid

```bash
interface GigabitEthernet0/0.10
 ip access-group NET-INTERNET-SOME-IN in
 ip access-list extended NET-INTERNET-SOME-IN

 remark  --- PERMIT MULTICAST ---
 permit ip 192.168.17.0 0.0.0.255 224.0.0.0 0.0.0.255
 remark --- PERMIT DHCP ---
 permit udp 192.168.17.0 0.0.0.255 any eq 67
 permit udp any eq 68 192.168.17.0 0.0.0.255 eq 67

 remark --- PERMIT DNS TO INTERNAL SERVER ---
 permit udp 192.168.17.0 0.0.0.255 host 192.168.18.100 eq 53
 permit tcp 192.168.17.0 0.0.0.255 host 192.168.18.100 eq 53

 remark --- PERMIT INTER-VLAN 10 ---
 permit ip 192.168.17.0 0.0.0.255 192.168.1.0 0.0.0.255

 remark --- DENY ACCESS TO INTERNAL RFC1918 ---
 deny ip 192.168.17.0 0.0.0.255 192.168.18.0 0.0.0.255
 deny ip 192.168.17.0 0.0.0.255 192.168.19.0 0.0.0.255
 deny ip 192.168.17.0 0.0.0.255 192.168.20.0 0.0.0.255
 deny ip 192.168.17.0 0.0.0.255 192.168.99.0 0.0.0.255
 deny ip 192.168.17.0 0.0.0.255 192.168.1.0 0.0.7.255

 remark --- PERMIT ANY ---
 permit ip any any

... (otros bloques de interfaces Gi0/0.20, 30, 40, 99 de Madrid)
```
