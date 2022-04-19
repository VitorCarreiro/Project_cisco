**Interfaces**
```
interface Serial0/1/1
 ip address 2.20.0.2 255.255.255.252
 ip nat outside

interface GigabitEthernet0/0
 ip address 172.16.20.2 255.255.255.0
 ip nat inside
```

**NAT overload and ACL**

```
ip nat inside source list 1 interface Serial0/1/1 overload

access-list 1 permit 172.16.0.0 0.15.255.255
access-list 1 permit 192.168.0.0 0.0.255.255
access-list 1 permit 10.0.0.0 0.255.255.255
```
**VPN**
```
interface Tunnel101
 ip address 192.168.20.6 255.255.255.252
 tunnel source Serial0/1/1
 tunnel destination 2.20.0.1

crypto map UMAPA 10 ipsec-isakmp
set peer 2.20.0.1
match address 100

access-list 100 permit gre host 2.20.0.2 host 2.20.0.1

int s0/1/1
crypto map UMAPA

crypto isakmp key Passw0rd address 2.20.0.1
crypto map UMAPA 10 ipsec-isakmp 
crypto ipsec transform-set TRANS ah-sha-hmac esp-aes 256 esp-sha-hmac

crypto map UMAPA 10 ipsec-isakmp 
set transform-set TRANS

crypto isakmp policy 10
authentication pre-share
encr aes 256
lifetime 10000
group 5
```
**OSPF and EIGRP**
```
router eigrp 100
 network 192.168.20.4 0.0.0.3
 network 172.16.20.0 0.0.0.255
 network 172.17.20.0 0.0.0.255
 network 172.18.20.0 0.0.0.255

router ospf 100
 redistribute eigrp 100 subnets 
 network 192.168.20.0 0.0.0.3 area 0
 network 172.16.20.0 0.0.0.255 area 0
 network 172.18.20.0 0.0.0.255 area 0
 network 172.17.20.0 0.0.0.255 area 0
 
 ```
 
