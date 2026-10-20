# Lab 3: DHCP and OSPF (Three-Router Network)

## Objective
Build a three-router triangle. Give the PCs addresses with DHCP and connect all networks with single-area OSPF (no static routes). Prove OSPF reroutes around a failed link.

## Topology
```
  PC1, PC2 ── SW1 ── R1 ═══════ R2 ── SW2 ── PC3, PC4
                      ║    ╲    ║
                      ╚══════ R3 ── SW3 ── Server0
```
R1, R2 and R3 are all connected to each other.

## Connections
| From | Port | To | Port | Cable |
|---|---|---|---|---|
| R1 | Gi0/1 | R2 | Gi0/1 | Cross-over |
| R1 | Gi0/2 | R3 | Gi0/1 | Cross-over |
| R2 | Gi0/2 | R3 | Gi0/2 | Cross-over |
| R1, R2, R3 | Gi0/0 | SW1, SW2, SW3 | Gi0/1 | Straight-through |
| PC1, PC2 | Fa0 | SW1 | Fa0/1, Fa0/2 | Straight-through |
| PC3, PC4 | Fa0 | SW2 | Fa0/1, Fa0/2 | Straight-through |
| Server0 | Fa0 | SW3 | Fa0/1 | Straight-through |

## Addressing
| Device | Interface | IP | Mask |
|---|---|---|---|
| R1 | Gi0/0 | 192.168.10.1 | 255.255.255.0 |
| R1 | Gi0/1 | 10.0.12.1 | 255.255.255.252 |
| R1 | Gi0/2 | 10.0.13.1 | 255.255.255.252 |
| R2 | Gi0/0 | 192.168.20.1 | 255.255.255.0 |
| R2 | Gi0/1 | 10.0.12.2 | 255.255.255.252 |
| R2 | Gi0/2 | 10.0.23.1 | 255.255.255.252 |
| R3 | Gi0/0 | 192.168.30.1 | 255.255.255.0 |
| R3 | Gi0/1 | 10.0.13.2 | 255.255.255.252 |
| R3 | Gi0/2 | 10.0.23.2 | 255.255.255.252 |
| Server0 | NIC | 192.168.30.10 | 255.255.255.0 (gw 192.168.30.1) |
| PC1 to PC4 | NIC | DHCP | |

## DHCP
| Router | Pool | Network | Gateway | DNS | Excluded |
|---|---|---|---|---|---|
| R1 | LAN_A | 192.168.10.0/24 | 192.168.10.1 | 192.168.30.10 | .1 - .10 |
| R2 | LAN_B | 192.168.20.0/24 | 192.168.20.1 | 192.168.30.10 | .1 - .10 |

## Tasks
1. Basic hardening on all six devices.
2. Configure router interfaces and Server0.
3. DHCP: exclude addresses first, then create the pools on R1 and R2.
4. OSPF process 1, area 0 on all routers, with manual router IDs (1.1.1.1, 2.2.2.2, 3.3.3.3).
5. Advertise every directly connected network with a wildcard mask.
6. Set LAN-facing interfaces passive.
7. Failover test: tracert PC1 to Server0, shut R1 Gi0/2, tracert again, restore.
8. Save the configuration.

## Verification
```
show ip dhcp binding
show ip ospf neighbor
show ip route
show ip protocols
show running-config | section dhcp
ipconfig /all (on a PC)
tracert 192.168.30.10 (before, during, after the link failure)
```

## Success Criteria
- [x] PCs get addresses with gateway and DNS, none in .1 - .10
- [x] Both neighbours `FULL` on every router
- [x] R1 learns `O` routes to LAN B and LAN C
- [x] Every PC pings Server0
- [x] Traffic reroutes R1 -> R2 -> R3 when the direct link fails
- [x] No static routes

## Lessons
- `network` statements must cover every interface you want advertised, including LANs.
- A passive interface is still advertised. Passive only stops hellos.
- OSPF cost adds across links. Equal-cost paths are both used (ECMP).
- A static route (AD 1) overrides OSPF (AD 110). A next hop must be a router, not a PC.
- Exclude DHCP addresses before creating the pool, and verify the pool has `default-router` and `dns-server`.
