**SW7**
```
int f0/15
sw mo tr

int range f0/11-12
sw mo tr
channel-protocol lacp
channel-group 1 mode active

vtp domain enta.pt
vtp mode server
vtp password Passw0rd

vlan 10
name V10
vlan 20
name V20
vlan 30
name V30

spanning-tree vlan 10 root primary
spanning-tree vlan 20 root secondary
spanning-tree vlan 30 root secondary
```
