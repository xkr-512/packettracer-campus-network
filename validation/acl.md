# ACL validation (VLAN10 USERS)

An extended ACL is applied on R1 inbound on the VLAN10 sub-interface (g0/0.10). The goal is deny-by-default from USERS, while keeping only the required services.

## Policy (VLAN10 -> internal)

Allowed:
- DNS to the INFRA server (192.168.20.10) on TCP/UDP 53
- Web access to the DMZ server (192.168.30.10) on TCP 80/443

Blocked:
- Any other traffic from VLAN10 to VLAN20 / VLAN30 / VLAN99 (example: ICMP/ping)

## Evidence (R1 — `show access-lists`)
~~~text
Extended IP access list VLAN10-FILTER
    10 permit udp 192.168.10.0 0.0.0.255 host 192.168.20.10 eq domain
    20 permit tcp 192.168.10.0 0.0.0.255 host 192.168.20.10 eq domain
    30 permit tcp 192.168.10.0 0.0.0.255 host 192.168.30.10 eq www
    40 permit tcp 192.168.10.0 0.0.0.255 host 192.168.30.10 eq 443
    50 deny ip 192.168.10.0 0.0.0.255 192.168.20.0 0.0.0.255
    60 deny ip 192.168.10.0 0.0.0.255 192.168.30.0 0.0.0.255
    70 deny ip 192.168.10.0 0.0.0.255 192.168.99.0 0.0.0.255
~~~

## Result
From a VLAN10 client:
- `http://web.dmz.lan` works (DNS + HTTP allowed)
- direct ping to 192.168.20.10 / 192.168.30.10 is blocked (as expected)