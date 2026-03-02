# Addressing notes (campus + branch)

This lab models a small campus site with centralized services (DHCP/DNS) and a small branch site connected over a point-to-point WAN link. OSPF is used to exchange routes between both sites.

## Campus (R1 + SW1)
- VLAN10 USERS: 192.168.10.0/24 (GW 192.168.10.1) — PC1 .11 / PC2 .12 / PC3 .13
- VLAN20 INFRA: 192.168.20.0/24 (GW 192.168.20.1) — DNS/DHCP server .10
- VLAN30 DMZ: 192.168.30.0/24 (GW 192.168.30.1) — Web server .10
- VLAN99 ADMIN: 192.168.99.0/24 (GW 192.168.99.1) — Admin PC .10

## Branch (R2 + SW2)
- VLAN110 BRANCH-USERS: 192.168.110.0/24 (GW 192.168.110.1) — BR-PC1 .11 / BR-PC2 .12

## WAN transit (R1–R2)
- 10.0.0.0/30 — R1 10.0.0.1, R2 10.0.0.2 (OSPF area 0)