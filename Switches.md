**SW7**
```

int f0/3
sw mo trunk

int f0/4
sw mo trunk

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
**SW9**
```
int range f0/11-12
sw mo tr
channel-protocol lacp
channel-group 1 mode active

int f0/13
sw mo tr

vtp domain enta.pt
vtp mode server
vtp password Passw0rd

spanning-tree vlan 20 root primary
spanning-tree vlan 30 root primary
spanning-tree vlan 10 root secondary
```
**SW11**
```
int range f0/11-12
sw mo tr
channel-protocol lacp
channel-group 1 mode active
int f0/13
sw mo tr
int f0/4
sw mo acc
sw acc vlan 30
int f0/5
sw mo acc
sw voice vlan 20

vtp domain enta.pt
vtp mode client
vtp password Passw0rd
```
**SW14**
```
int range f0/11-12
sw mo tr
channel-protocol lacp
channel-group 1 mode active
int f0/15
sw mo tr
int f0/4
sw mo acc
sw acc vlan 10

vtp domain enta.pt
vtp mode client
vtp password Passw0rd
```
