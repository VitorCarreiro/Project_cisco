**NAT and ACL**
```
ip nat inside source list 1 interface Serial0/0/0 overload

access-list 1 permit 10.0.0.0 0.255.255.255
access-list 1 permit 172.16.0.0 0.15.255.255
access-list 1 permit 192.168.0.0 0.0.255.255
```

**DHCP**
```
ip dhcp excluded-address 172.16.20.1
ip dhcp excluded-address 172.16.20.253
ip dhcp excluded-address 172.18.20.253
ip dhcp excluded-address 172.18.20.1

ip dhcp pool inside
 network 172.18.20.0 255.255.255.0
 default-router 172.18.20.253
 option 150 ip 172.18.0.1
 dns-server 8.8.8.8
 ```
 **Telephony Service**
 ```
 telephony-service
 max-ephones 10
 max-dn 10
 ip source-address 172.18.20.1 port 2000
 auto assign 1 to 10
 
 ephone-dn 1
 number 200
ephone-dn 2
 number 201
 
 ```
 **For VPN**
 ```
 crypto map OMAPA 10 ipsec-isakmp 
set peer 1.20.0.1
match address 100

access-list 100 permit gre host 1.20.0.2 host 1.20.0.1

int s0/1/1
crypto map OMAPA

crypto isakmp key Passw0rd address 1.20.0.1
crypto map OMAPA 10 ipsec-isakmp 
crypto ipsec transform-set TRANS ah-sha-hmac esp-aes 256 esp-sha-hmac

crypto map OMAPA 10 ipsec-isakmp 
set transform-set TRANS 

crypto isakmp policy 10
authentication pre-share
encr aes 256
lifetime 10000
group 5
```
**EIGRP and OSPF**
```
router eigrp 100
 redistribute static 
 network 192.168.20.0 0.0.0.3
 network 172.16.20.0 0.0.0.255
 network 172.18.20.0 0.0.0.255
 network 172.17.20.0 0.0.0.255
 no auto-summary

router ospf 100
 log-adjacency-changes
 redistribute eigrp 100 subnets 
 network 192.168.20.0 0.0.0.3 area 0
 network 172.16.20.0 0.0.0.255 area 0
 network 172.18.20.0 0.0.0.255 area 0
 network 172.17.20.0 0.0.0.255 area 0
```
