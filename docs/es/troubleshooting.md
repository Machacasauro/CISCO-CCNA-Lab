# 🛠️ Troubleshooting - Red Empresarial CCNA Multisede

Este documento describe los problemas encontrados durante el desarrollo del laboratorio y las soluciones aplicadas.



## ⚡ Problemas Comunes y Soluciones

### 1. ❌ OSPF no establece vecinos entre R1 y R2 (a través del GRE)
**Síntoma**: Mensajes como:
```
%OSPF-5-ADJCHG: Neighbor Down: Dead timer expired
```
**Causa**:
- Tunnel0 no estaba anunciado en OSPF.
- El router R1 usaba loopback como `tunnel source` pero no se enrutaba correctamente.

**Solución**:
```bash
router ospf 1
 network 10.0.0.0 0.0.0.3 area 0
```
Asegurarse también de que el `tunnel destination` sea enrutable y esté en la tabla de rutas.



### 2. ⚠️ DHCP no asigna IPs
**Causa**:
- Excluded-addresses mal definidos.
- No hay default-router configurado.

**Solución**:
Revisar `ip dhcp excluded-address` y que el `default-router` pertenezca a la VLAN correspondiente.



### 3. ⚠️ STP bloqueando tráfico entre switches
**Causa**:
- Múltiples enlaces sin control.

**Solución**:
```bash
spanning-tree mode rapid-pvst
spanning-tree vlan X root primary
```
Usar EtherChannel correctamente y evitar loops.



### 4. ❌ BGP no forma vecinos
**Causa**:
- IP GRE mal configurada
- No hay conectividad hacia la IP de destino

**Solución**:
Verificar:
```bash
ping 10.0.0.2
router bgp 65001
 neighbor 10.0.0.2 remote-as 65002
```



### 5. ❌ SSH no funciona
**Causa**: Faltaban comandos en `line vty`
**Solución**:
```bash
line vty 0 4
 login local
 transport input ssh
```



### 6. ⚡ NAT bloqueando GRE/BGP
**Causa**: NAT sobre interfaz del tunnel
**Solución**: Excluir el tráfico GRE/BGP del NAT (no necesario en PT, pero buena práctica en entornos reales)



## ✅ Recomendaciones Finales
- Verifica siempre conectividad IP antes de configurar protocolos de enrutamiento.
- Usa `show ip route`, `show ip ospf neighbor`, `show ip bgp summary` para diagnóstico.
- Documenta siempre tus cambios y respaldos por dispositivo.

