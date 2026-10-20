# CCNA Packet Tracer Labs

Hands-on CCNA practice labs built in Cisco Packet Tracer to refresh and strengthen my networking skills. Each lab has an objective, topology, addressing table, tasks, verification commands, success criteria, and the problems I hit and fixed.

## Labs

| # | Lab | Skills |
|---|---|---|
| 1 | [Basic Connectivity](labs/lab-1-basic-connectivity/) | Device hardening, interface addressing, static routing, ping/tracert |
| 2 | [VLANs, Trunking, Router-on-a-Stick](labs/lab-2-vlans-router-on-a-stick/) | VLANs, access ports, 802.1Q trunk, native VLAN, subinterfaces |
| 3 | [DHCP and OSPF](labs/lab-3-dhcp-ospf/) | DHCP pools and exclusions, OSPF single-area, wildcard masks, passive interfaces, failover |

See [troubleshooting-log.md](troubleshooting-log.md) for the faults found in each lab and how they were fixed.

## Repository layout

```
labs/
  lab-N-name/
    README.md      objective, topology, tables, tasks, verification, lessons
    configs.txt    reference configuration for every device
    *.pkt          Packet Tracer file (add yours here)
troubleshooting-log.md
```

## How to use
1. Open the lab README and build the topology in Packet Tracer.
2. Try every task yourself using the verification commands.
3. Compare against `configs.txt` only after you have tried.

## Next
- Lab 4: Internet edge (default route, NAT overload, extended ACLs, SSH)
- Later: STP, EtherChannel, port security, timed practical exams

> Lab passwords (`class`, `cisco`) are practice values only.
