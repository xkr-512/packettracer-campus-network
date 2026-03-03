# Test plan (quick checks)

I use this checklist after each major change to make sure the lab still behaves as expected.

## L2 (switching)
- SW1: VLANs and port membership look right (`show vlan brief`)
- SW1: trunk to R1 is up and only carries 10/20/30/99 (`show interfaces trunk`)

## L3 (routing)
- From a VLAN10 client: can reach its gateway (192.168.10.1)
- Inter-VLAN baseline (before ACL): VLAN10 can reach INFRA (192.168.20.10) and DMZ (192.168.30.10)

## OSPF (campus <> branch)
- R1/R2 neighbor is FULL (`show ip ospf neighbor`)
- R1 learns 192.168.110.0/24, R2 learns 192.168.10/20/30/99 (`show ip route ospf`)
- End-to-end: a campus client can ping a branch client

## Services
- DHCP works in VLAN10 and VLAN110 (clients get IP + GW + DNS = 192.168.20.10)
- DNS resolves `web.dmz.lan` and `www.dmz.lan`
- HTTP to the DMZ web server works from VLAN10

## ACL (deny-by-default from VLAN10)
- DNS (53) to 192.168.20.10 still works
- Web (80/443) to 192.168.30.10 still works
- Direct ping to 192.168.20.10 / 192.168.30.10 is blocked
- ACL counters increase (`show access-lists`)