# Verification Results

The following results were captured from the completed Cisco Packet Tracer simulation.

## VLAN membership

Both switches showed the intended access-port assignments. The following captured output was recorded before the uplinks were converted from access ports to trunks:

```text
VLAN Name                             Status    Ports
---- -------------------------------- --------- -------------------------------
1    default                          active    Fa0/4, Fa0/5, Fa0/6, Fa0/7
                                                Fa0/8, Fa0/9, Fa0/10, Fa0/11
                                                Fa0/12, Fa0/13, Fa0/14, Fa0/15
                                                Fa0/16, Fa0/17, Fa0/18, Fa0/19
                                                Fa0/20, Fa0/21, Fa0/22, Fa0/23
                                                Fa0/24, Gig0/1, Gig0/2
10   IT                               active    Fa0/1
20   HR                               active    Fa0/2
30   Sales                            active    Fa0/3
```

Trunk interfaces do not appear under an access VLAN in `show vlan brief`; they were validated separately.

## Switch1 trunk verification

```text
Port        Mode         Encapsulation  Status        Native vlan
Gig0/1      on           802.1q         trunking      1
Gig0/2      on           802.1q         trunking      1

Port        Vlans allowed and active in management domain
Gig0/1      1,10,20,30
Gig0/2      1,10,20,30

Port        Vlans in spanning tree forwarding state and not pruned
Gig0/1      1,10,20,30
Gig0/2      1,10,20,30
```

## Switch2 trunk verification

```text
Port        Mode         Encapsulation  Status        Native vlan
Gig0/1      on           802.1q         trunking      1

Port        Vlans allowed and active in management domain
Gig0/1      1,10,20,30

Port        Vlans in spanning tree forwarding state and not pruned
Gig0/1      1,10,20,30
```

## Router interface verification

```text
Interface              IP-Address      OK? Method Status                Protocol
GigabitEthernet0/0     unassigned      YES unset  up                    up
GigabitEthernet0/0.10  192.168.10.1    YES manual up                    up
GigabitEthernet0/0.20  192.168.20.1    YES manual up                    up
GigabitEthernet0/0.30  192.168.30.1    YES manual up                    up
GigabitEthernet0/1     unassigned      YES unset  administratively down down
GigabitEthernet0/2     unassigned      YES unset  administratively down down
Vlan1                  unassigned      YES unset  administratively down down
```

## Same-VLAN connectivity across switches

PC1 (`192.168.10.11`) successfully reached PC4 (`192.168.10.12`):

```text
Pinging 192.168.10.12 with 32 bytes of data:

Reply from 192.168.10.12: bytes=32 time<1ms TTL=128
Reply from 192.168.10.12: bytes=32 time<1ms TTL=128
Reply from 192.168.10.12: bytes=32 time<1ms TTL=128
Reply from 192.168.10.12: bytes=32 time<1ms TTL=128

Packets: Sent = 4, Received = 4, Lost = 0 (0% loss)
```

## Inter-VLAN connectivity

PC1 in VLAN 10 successfully reached PC2 in VLAN 20:

```text
Pinging 192.168.20.11 with 32 bytes of data:

Request timed out.
Reply from 192.168.20.11: bytes=32 time<1ms TTL=127
Reply from 192.168.20.11: bytes=32 time=4ms TTL=127
Reply from 192.168.20.11: bytes=32 time<1ms TTL=127

Packets: Sent = 4, Received = 3, Lost = 1 (25% loss)
```

PC1 in VLAN 10 successfully reached PC6 in VLAN 30 across both switches:

```text
Pinging 192.168.30.12 with 32 bytes of data:

Request timed out.
Reply from 192.168.30.12: bytes=32 time<1ms TTL=127
Reply from 192.168.30.12: bytes=32 time<1ms TTL=127
Reply from 192.168.30.12: bytes=32 time=1ms TTL=127

Packets: Sent = 4, Received = 3, Lost = 1 (25% loss)
```

The first request timed out while Packet Tracer populated the required ARP information. The subsequent replies confirmed successful routing, and TTL 127 was consistent with one routed hop.
