# Lab 1: Basic Connectivity (Diagnostic)

## Objective
Configure two LANs connected by two routers. Make every device reach every other device.

## Topology
```
 PC1 ──┐                                  ┌── PC3
       ├── SW1 ── R1 ══════════ R2 ── SW2 ┤
 PC2 ──┘                                  └── PC4
```

## Connections
| From | Port | To | Port | Cable |
|---|---|---|---|---|
| PC1 / PC2 | Fa0 | SW1 | Fa0/1, Fa0/2 | Straight-through |
| SW1 | Gi0/1 | R1 | Gi0/0 | Straight-through |
| R1 | Gi0/1 | R2 | Gi0/1 | Cross-over |
| R2 | Gi0/0 | SW2 | Gi0/1 | Straight-through |
| PC3 / PC4 | Fa0 | SW2 | Fa0/1, Fa0/2 | Straight-through |

## Addressing
| Device | Interface | IP | Mask | Gateway |
|---|---|---|---|---|
| R1 | Gi0/0 | 192.168.10.1 | 255.255.255.0 | |
| R1 | Gi0/1 | 10.0.0.1 | 255.255.255.252 | |
| R2 | Gi0/1 | 10.0.0.2 | 255.255.255.252 | |
| R2 | Gi0/0 | 192.168.20.1 | 255.255.255.0 | |
| SW1 | VLAN 1 | 192.168.10.2 | 255.255.255.0 | 192.168.10.1 |
| SW2 | VLAN 1 | 192.168.20.2 | 255.255.255.0 | 192.168.20.1 |
| PC1 | NIC | 192.168.10.11 | 255.255.255.0 | 192.168.10.1 |
| PC2 | NIC | 192.168.10.12 | 255.255.255.0 | 192.168.10.1 |
| PC3 | NIC | 192.168.20.11 | 255.255.255.0 | 192.168.20.1 |
| PC4 | NIC | 192.168.20.12 | 255.255.255.0 | 192.168.20.1 |

## Tasks
1. Hostname, `no ip domain-lookup`, `enable secret`, console password with `login`, banner, `service password-encryption` on all four devices.
2. Configure router interfaces and `no shutdown`.
3. Configure switch management IPs (SVI) and default gateways.
4. Configure the PCs.
5. Configure static routes between the two LANs.
6. Save the configuration.

## Verification
```
show ip interface brief
show ip route
show running-config | section line
ping / tracert from PC1 to PC2, PC3, PC4
```

## Success Criteria
- [x] All router interfaces `up/up`
- [x] PC1 pings PC2, PC3 and PC4
- [x] `show ip route` on R1 shows an `S` route to 192.168.20.0/24
- [x] Configuration saved

## Lessons
- `TTL=126` on a ping to the far LAN means the packet crossed two routers (128 - 2).
- A PC only needs a correct **default gateway** to reach other networks.
- A route destination is a **network address**, and the next hop is a **neighbouring router**.
- Routing must work in both directions.
