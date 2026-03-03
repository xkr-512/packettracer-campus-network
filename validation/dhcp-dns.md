# DHCP + DNS validation

DHCP and DNS are centralized on the INFRA server (192.168.20.10). Clients in the Campus USERS VLAN (10) and Branch USERS VLAN (110) use DHCP.

## DHCP (samples observed in Packet Tracer)

VLAN10 (Campus USERS):
- PC1: 192.168.10.100/24 — GW 192.168.10.1 — DNS 192.168.20.10
- PC2: 192.168.10.101/24 — GW 192.168.10.1 — DNS 192.168.20.10
- PC3: 192.168.10.102/24 — GW 192.168.10.1 — DNS 192.168.20.10

VLAN110 (Branch USERS):
- BR-PC1: 192.168.110.100/24 — GW 192.168.110.1 — DNS 192.168.20.10
- BR-PC2: 192.168.110.101/24 — GW 192.168.110.1 — DNS 192.168.20.10

## DNS (internal)

DNS records are hosted on 192.168.20.10:
- A: `web.dmz.lan` -> 192.168.30.10
- CNAME: `www.dmz.lan` -> `web.dmz.lan`

## Evidence (DHCP relay)

R1 relays DHCP for VLAN10 to the INFRA server:
~~~text
interface GigabitEthernet0/0.10
 ip helper-address 192.168.20.10
~~~

R2 relays DHCP for VLAN110 to the INFRA server:
~~~text
interface GigabitEthernet0/0
 ip helper-address 192.168.20.10
~~~

## Quick checks
- Name resolution works from VLAN10 clients (ping / browser using `web.dmz.lan` and `www.dmz.lan`).
- Branch clients obtain DHCP leases via relay and reach DNS over the WAN link (routing provided by OSPF).