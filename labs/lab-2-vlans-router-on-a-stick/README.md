# Lab 2: VLANs, Trunking and Router-on-a-Stick

## Objective
Split one switch into VLANs 10 and 20 (plus management VLAN 99), then route between them with one router interface using subinterfaces.

## Topology
```
 PC1 (Sales) ──┐
 PC2 (Sales) ──┤
               ├── SW1 ══ 802.1Q trunk ══ R1 (Gi0/0.10 / .20 / .99)
 PC3 (IT)    ──┤
 PC4 (IT)    ──┘
```

## Connections
| From | Port | To | Port |
|---|---|---|---|
| PC1, PC2 | Fa0 | SW1 | Fa0/1, Fa0/2 |
| PC3, PC4 | Fa0 | SW1 | Fa0/3, Fa0/4 |
| SW1 | Gi0/1 | R1 | Gi0/0 |

## VLANs
| ID | Name | Subnet | Ports |
|---|---|---|---|
| 10 | Sales | 192.168.10.0/24 | Fa0/1-2 |
| 20 | IT | 192.168.20.0/24 | Fa0/3-4 |
| 99 | Management | 192.168.99.0/24 | SVI only (native VLAN) |

## Addressing
| Device | Interface | IP | Mask | Gateway |
|---|---|---|---|---|
| R1 | Gi0/0.10 | 192.168.10.1 | 255.255.255.0 | |
| R1 | Gi0/0.20 | 192.168.20.1 | 255.255.255.0 | |
| R1 | Gi0/0.99 | 192.168.99.1 | 255.255.255.0 | |
| SW1 | VLAN 99 | 192.168.99.2 | 255.255.255.0 | 192.168.99.1 |
| PC1 | NIC | 192.168.10.11 | 255.255.255.0 | 192.168.10.1 |
| PC2 | NIC | 192.168.10.12 | 255.255.255.0 | 192.168.10.1 |
| PC3 | NIC | 192.168.20.11 | 255.255.255.0 | 192.168.20.1 |
| PC4 | NIC | 192.168.20.12 | 255.255.255.0 | 192.168.20.1 |

## Tasks
1. Basic hardening on R1 and SW1.
2. Create VLANs 10, 20, 99 with names. Assign access ports.
3. Management SVI on VLAN 99 and default gateway on SW1.
4. Trunk on SW1 Gi0/1 with native VLAN 99.
5. `no shutdown` on R1 Gi0/0. Create subinterfaces with `encapsulation dot1Q`. Mark `.99` as `native`.
6. Configure PCs and save.

## Verification
```
show vlan brief
show interfaces trunk
show ip interface brief
show ip route
ping PC1 -> PC2, PC3, 192.168.99.2
```

## Success Criteria
- [x] VLAN 10, 20, 99 present with the correct ports
- [x] Gi0/1 `trunking`, native VLAN 99, VLANs 1,10,20,99 active
- [x] Three subinterfaces `up/up`
- [x] PC1 pings PC3 through the router
- [x] Shutting R1 Gi0/0 breaks inter-VLAN traffic, and `no shutdown` restores it

## Lessons
- VLANs separate broadcast domains at Layer 2. Only a Layer 3 device connects them.
- A down router interface means a dead trunk. Check Layer 1 first.
- Native VLAN must match on both ends of the trunk.
- All inter-VLAN traffic crosses one physical link, which is the weakness of router-on-a-stick.
