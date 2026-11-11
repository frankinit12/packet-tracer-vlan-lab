# Verification Results

Validated in Cisco Packet Tracer 9.0.1 on November 11, 2026.

## DHCP

On PC0 (VLAN 10), Desktop → IP Configuration → DHCP, then Command Prompt:

```text
C:\>ipconfig /renew
IP Address:      192.168.10.21
Subnet Mask:     255.255.255.0
Default Gateway: 192.168.10.1
DNS Server:      8.8.8.8
```

On PC2 (VLAN 20), DHCP renewal returned `192.168.20.21/24`, gateway `192.168.20.1`, and DNS `8.8.8.8`. Router `show ip dhcp binding` showed:

```text
192.168.20.21    0030.F29A.66C5           --                     Automatic
```

## Connectivity

PC0 to its default gateway (`ping 192.168.10.1`): 4 sent, 4 received, 0 lost.

PC0 across VLANs to PC2 (`ping 192.168.20.21`): 4 sent, 4 received, 0 lost. Replies used TTL 127, confirming the router forwarded the traffic.

Router `show ip interface brief` showed G0/0.10 (`192.168.10.1`) and G0/0.20 (`192.168.20.1`) up/up. Switch spanning tree showed Fa0/5 forwarding in VLANs 10 and 20.
