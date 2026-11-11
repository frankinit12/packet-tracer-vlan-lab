# Verification Results

Recorded in Cisco Packet Tracer 9.0.1 on November 11, 2026.

## DHCP

On PC0 (VLAN 10), after selecting Desktop → IP Configuration → DHCP and running `ipconfig /renew`:

```text
IP address:      192.168.10.21
Subnet mask:     255.255.255.0
Default gateway: 192.168.10.1
DNS server:      8.8.8.8
```

PC2 (VLAN 20) received 192.168.20.21/24, gateway 192.168.20.1, and DNS 8.8.8.8. Router `show ip dhcp binding` listed 192.168.20.21 as an automatic lease. PC1 and PC3 were not checked.

## Connectivity and interface status

- PC0 to its gateway (`ping 192.168.10.1`): 4 sent, 4 received, 0 lost.
- PC0 to PC2 across VLANs (`ping 192.168.20.21`): 4 sent, 4 received, 0 lost; replies had TTL 127.
- Router G0/0.10 (192.168.10.1) and G0/0.20 (192.168.20.1) showed up/up.
- Switch Fa0/5 was forwarding in VLANs 10 and 20.

Port security was configured on the access ports. A violation test was not recorded.
