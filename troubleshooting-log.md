# Troubleshooting Log

| Lab | Symptom | Cause | Fix | Lesson |
|---|---|---|---|---|
| 1 | Ping to the far router link timed out, near side worked | Return path missing. R2 had no route to LAN A, so the reply was dropped | Static route on each router | Routing must work in both directions |
| 1 | `%Inconsistent address and mask` | Host address used as the route destination | Use the network address (`192.168.10.0`, not `.2`) | A route destination is a network, not a host |
| 1 | Banner showed only part of its text | The delimiter character also appeared inside the banner text | Choose a delimiter not used in the text | Check delimiters |
| 2 | `show interfaces trunk` was empty | Router Gi0/0 was administratively down, so no link | `no shutdown` on the physical interface | Check Layer 1 first |
| 2 | Native VLAN showed 5 instead of 99 | Number copied from a generic example | `switchport trunk native vlan 99` | Compare against the lab tables, not the example |
| 2 | Ping between VLANs timed out during the shutdown test | Router was the only path between the VLANs | Expected result | A controlled failure proves the traffic path |
| 3 | R1 had no routes to LAN B or LAN C | No router advertised its own LAN in OSPF | Add a `network` statement for each LAN (wildcard `0.0.0.255`) | Check `show ip protocols` on the owner of the network |
| 3 | Passive interface and advertisement confused | Passive only stops hellos, it still advertises the network | Keep the `network` line and add `passive-interface` | Passive is not the same as not advertised |
| 3 | PC received no default gateway | Pool had `network` but no `default-router`. Duplicate pools overlapped | Complete the pool and delete the stray pools | Verify with `show running-config \| section dhcp` |
| 3 | DHCP handed out `.2` and `.3` | Exclusion covered only `.1` | `ip dhcp excluded-address 192.168.10.1 192.168.10.10` | Exclude a range, before creating the pool |
| 3 | Static route used a PC as next hop | Next hop must be a neighbouring router | Remove the route. Static AD 1 beats OSPF AD 110 | Static routes override OSPF |
| 3 | Path after link failure | OSPF rerouted R1, R2, R3 automatically | Expected result | Failover works through cost recalculation |
