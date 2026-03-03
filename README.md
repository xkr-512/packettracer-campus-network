# Packet Tracer Campus Network

A small “campus + branch” Packet Tracer build focused on the basics you actually use in real networks: VLAN segmentation, ROAS inter-VLAN routing, OSPF between sites, centralized DHCP/DNS, and a simple deny-by-default ACL policy.

The campus hosts the infra services (DHCP/DNS) and a DMZ web server. The branch is a single user VLAN connected over a point-to-point WAN link. Routing between sites is handled by OSPF (area 0).

## Topology (high level)
**Campus (Site A)**
- VLAN10 USERS (clients via DHCP)
- VLAN20 INFRA (DHCP + DNS server)
- VLAN30 DMZ (web server)
- VLAN99 ADMIN (admin workstation)
- ROAS on R1 (sub-interfaces per VLAN)

**Branch (Site B)**
- VLAN110 BRANCH-USERS (clients via DHCP)
- Routed by R2

**WAN**
- R1 ↔ R2 transit link: 10.0.0.0/30
- OSPF area 0 between R1 and R2

## What this lab demonstrates
- L2: VLANs, access ports, trunk to ROAS router
- L3: Inter-VLAN routing on R1 (ROAS) + routed branch on R2
- Dynamic routing: OSPF adjacency and route exchange (area 0)
- Services:
  - DHCP centralized on the INFRA server + DHCP relay (ip helper) for remote VLANs
  - Internal DNS with A + CNAME records for the DMZ web server
- Basic security:
  - Deny-by-default from VLAN10 (USERS) using an extended ACL
  - Explicit exceptions for DNS (53) to INFRA and HTTP/HTTPS (80/443) to DMZ

## Repo contents
- `packet-tracer/campus-network.pkt` — the full topology
- `topology/addressing-plan.md` — addressing notes + DHCP pools
- `configs/` — device outputs (R1/R2/SW1/SW2)
- `validation/` — short validation notes (OSPF / DHCP+DNS / ACL)
- `tests/test-plan.md` — quick checklist used to validate changes

## How to open
1. Open `packet-tracer/campus-network.pkt` in Cisco Packet Tracer.
2. Verify the expected state:
   - OSPF neighbor is FULL between R1 and R2
   - VLAN10 and VLAN110 clients are set to DHCP
   - DNS server is `192.168.20.10`
3. Run the checks listed in `tests/test-plan.md`.

## Validation highlights
- OSPF: adjacency FULL and routes learned on both sides  
  → see `validation/ospf.md`
- DHCP/DNS: VLAN10 + VLAN110 leases via relay, internal DNS resolves DMZ records  
  → see `validation/dhcp-dns.md`
- ACL: VLAN10 users can resolve DNS and browse DMZ web, but direct access to INFRA/DMZ is blocked  
  → see `validation/acl.md`

## Notes
- Client addressing is DHCP-based by design, so client IPs may change within the configured pools.
- This is a Packet Tracer lab: it’s meant for learning and demonstrating concepts, not production deployment.
