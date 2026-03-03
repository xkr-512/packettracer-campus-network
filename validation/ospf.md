# OSPF validation

Campus (R1) and Branch (R2) are connected over a /30 transit link (10.0.0.0/30). OSPF area 0 is used to exchange routes between both sites.

## What I checked
- R1 and R2 form an OSPF adjacency (FULL).
- R1 learns the Branch subnet (192.168.110.0/24) via OSPF.
- R2 learns Campus VLANs (10/20/30/99) via OSPF.

## Evidence (neighbors)

### R1 — `show ip ospf neighbor`
~~~text
Neighbor ID     Pri   State           Dead Time   Address         Interface
2.2.2.2           1   FULL/DR         00:00:30    10.0.0.2        GigabitEthernet0/1
~~~

### R2 — `show ip ospf neighbor`
~~~text
Neighbor ID     Pri   State           Dead Time   Address         Interface
1.1.1.1           1   FULL/BDR        00:00:36    10.0.0.1        GigabitEthernet0/1
~~~

## Evidence (OSPF routes)

### R1 learns Branch VLAN
~~~text
O    192.168.110.0/24 [110/2] via 10.0.0.2, 00:04:58, GigabitEthernet0/1
~~~

### R2 learns Campus VLANs
~~~text
O    192.168.10.0/24  [110/2] via 10.0.0.1, 00:11:12, GigabitEthernet0/1
O    192.168.20.0/24  [110/2] via 10.0.0.1, 00:11:12, GigabitEthernet0/1
O    192.168.30.0/24  [110/2] via 10.0.0.1, 00:11:12, GigabitEthernet0/1
O    192.168.99.0/24  [110/2] via 10.0.0.1, 00:11:12, GigabitEthernet0/1
~~~