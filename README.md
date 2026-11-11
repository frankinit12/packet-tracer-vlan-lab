# Two-VLAN Packet Tracer Lab

## Current build status

The Packet Tracer workspace contains a Cisco 2911 router, a 2960-24TT switch, and four PCs. Each PC is connected to the switch. The switch was configured and its running configuration was copied to NVRAM with `write memory`:

- VLAN 10 (`SALES`): Fa0/1 and Fa0/2
- VLAN 20 (`STAFF`): Fa0/3 and Fa0/4
- Access ports have sticky MAC learning and port-security violation mode `restrict`.
- Switch hostname: `SW1`; the enable secret is redacted in the published configuration examples.

The native `.pkt` file is saved in this folder. The four PCs are connected to the switch, and the router is cabled to Switch0 `Fa0/5`. Router subinterfaces for VLANs 10 and 20, DHCP pools, and the switch trunk on `Fa0/5` have been configured and saved. The router CLI confirms both subinterfaces are up/up. The switch CLI confirms VLAN 10 and VLAN 20 are active and Fa0/5 is an 802.1Q trunk; spanning tree reports Fa0/5 forwarding in both VLANs. Sticky MAC port security is configured on the four PC access ports.

**Verification passed in Packet Tracer:**

- PC0 received `192.168.10.21/24` from DHCP with gateway `192.168.10.1` and DNS `8.8.8.8`.
- PC2 received `192.168.20.21/24` from DHCP with gateway `192.168.20.1` and DNS `8.8.8.8`.
- Router `show ip dhcp binding` showed PC2's lease as an automatic binding.
- PC0 pinged its VLAN 10 gateway successfully (4/4 replies).
- PC0 pinged PC2 across VLANs successfully (4/4 replies, 0% loss).
- Router subinterfaces were up/up; switch spanning tree showed the trunk forwarding in VLANs 10 and 20.

See [`verification-results.md`](verification-results.md) for the commands and recorded results. [`topology.svg`](topology.svg) shows the lab layout.

PC0 and PC2 are set to DHCP in the saved Packet Tracer file. PC1 and PC3 remain available for additional DHCP-client checks.

## Addressing plan

| VLAN | Name | Network | Default gateway | DHCP range |
|---|---|---|---|---|
| 10 | SALES | 192.168.10.0/24 | 192.168.10.1 | 192.168.10.21–192.168.10.254 |
| 20 | STAFF | 192.168.20.0/24 | 192.168.20.1 | 192.168.20.21–192.168.20.254 |

## Rechecking the lab

Open the saved `.pkt` file. To repeat the checks, use these commands and steps:

1. On the router, verify:

   ```text
   show ip interface brief
   show ip dhcp pool
   show ip dhcp binding
   ```

2. On the switch, use `show vlan brief`, `show interfaces trunk`, and `show port-security interface fa0/1` (repeat for Fa0/2–Fa0/4 as needed).
3. On a client, choose Desktop → IP Configuration → DHCP. Confirm the address is in the matching subnet.
4. From Command Prompt, ping the client's default gateway and a client in the other VLAN. A successful cross-VLAN ping confirms routing.
5. To demonstrate that DHCP issued a lease, confirm the client appears in the router's `show ip dhcp binding` output.

## Security note

The enable secret is redacted from the published configuration examples. Port security limits each PC access port to one learned MAC address and restricts traffic from an unexpected MAC.

## Resume entry (after completing and verifying)

**Packet Tracer Network Lab** | [Month Year]

- Designed and configured a simulated network with two VLANs, DHCP, inter-VLAN routing, and switch port security.
- Documented the topology, IP addressing scheme, and configuration steps.
