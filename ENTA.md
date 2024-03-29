# Project_cisco ENTA

**Interfaces**
```
interface Serial0/0/0
 no shut
 ip address 1.20.0.1 255.255.255.252
 ip nat out
 
interface Serial0/0/1
 no shut
 ip address 2.20.0.1 255.255.255.252
 ip nat out
 ```

**Do DHCP to provide and assigns IP addresses**

```
ip dhcp excluded-address 10.0.20.1
ip dhcp excluded-address 10.1.20.1
ip dhcp excluded-address 10.2.20.1


ip dhcp pool V10
 network 10.0.20.0 255.255.255.0
 default-router 10.0.20.1
 option 150 ip 10.0.20.1
 dns-server 8.8.8.8
ip dhcp pool V20
 network 10.1.20.0 255.255.255.0
 default-router 10.1.20.1
 option 150 ip 10.1.20.1
 dns-server 8.8.8.8
ip dhcp pool V30
 network 10.2.20.0 255.255.255.0
 default-router 10.2.20.1
 option 150 ip 10.2.20.1
 dns-server 8.8.8.8
 
 ```
 
 **Make 2 Tunnels for VPN**
 
 ```
 
interface Tunnel100
 ip address 192.168.20.1 255.255.255.252
 tunnel source Serial0/0/0
 tunnel destination 1.20.0.2

interface Tunnel101
 ip address 192.168.20.5 255.255.255.252
 tunnel source Serial0/0/1
 tunnel destination 2.20.0.2
 
 crypto map OMAPA 10 ipsec-isakmp 
set peer 1.20.0.2
match address 100

access-list 100 permit gre host 1.20.0.1 host 1.20.0.2

int s0/0/0
crypto map OMAPA

crypto isakmp key Passw0rd address 1.20.0.2
crypto map OMAPA 10 ipsec-isakmp 
crypto ipsec transform-set TRANS ah-sha-hmac esp-aes 256 esp-sha-hmac

crypto map OMAPA 10 ipsec-isakmp 
set transform-set TRANS 

crypto isakmp policy 10
authentication pre-share
encr aes 256
lifetime 10000
group 5

crypto map UMAPA 10 ipsec-isakmp
set peer 2.20.0.2
match address 101

access-list 101 permit gre host 2.20.0.1 host 2.20.0.2

int s0/0/1
crypto map UMAPA

crypto isakmp key Passw0rd address 2.20.0.2
crypto map UMAPA 10 ipsec-isakmp 
crypto ipsec transform-set TRANS ah-sha-hmac esp-aes 256 esp-sha-hmac

crypto map UMAPA 10 ipsec-isakmp 
set transform-set TRANS
```

**Make sub-interfaces and give them an IP**
```
interface g0/0.10
 encapsulation dot1Q 10
 ip address 10.0.20.1 255.255.255.0
 
interface g0/0.20
 encapsulation dot1Q 20
 ip address 10.1.20.1 255.255.255.0

 
interface g0/0.30
 encapsulation dot1Q 30
 ip address 10.2.20.1 255.255.255.0
```
**For telephony-service**
 ```
telephony-service
max-ephones 10
max-dn 10
ip source-address 10.1.20.1 port 2000
auto assign 1 to 10
create cnf-files

ephone-dn 1
 number 100

ephone-dn 2
 number 101

ephone-dn 3
 number 102
 ```
**For NAT and ACL**
```
ip nat inside source list 1 interface Serial0/0/1 overload
ip nat inside source list 1 interface Serial0/0/0 overload

access-list 1 permit 10.0.0.0 0.255.255.255
access-list 1 permit 172.16.0.0 0.15.255.255
access-list 1 permit 192.168.0.0 0.0.255.255
```

**Use OSPF to distribute IP routing**
```
router ospf 100
 passive-interface g0/0
 passive-interface g0/1
 network 10.0.20.0 0.0.0.255 area 0
 network 10.1.20.0 0.0.0.255 area 0
 network 10.2.20.0 0.0.0.255 area 0
 network 192.168.20.0 0.0.0.3 area 0
 network 192.168.20.4 0.0.0.3 area 0
 ```
