# Информация с роутера router1
# Дата: 2026-04-02 13:58:42.154239
# IP: 10.0.0.1
--------------------------------------------------

=== show ip ospf neighbor ===

Neighbor ID     Pri   State           Dead Time   Address         Interface
2.2.2.2           0   FULL/  -        00:00:31    10.0.0.2        GigabitEthernet1/0

--------------------------------------------------

=== show mpls forwarding-table ===
Local      Outgoing   Prefix           Bytes Label   Outgoing   Next Hop    
Label      Label      or Tunnel Id     Switched      interface              
16         Pop Label  2.2.2.2/32       0             Gi1/0      10.0.0.2    
17         Pop Label  10.0.0.4/30      0             Gi1/0      10.0.0.2    
18         Pop Label  10.0.0.8/30      0             Gi1/0      10.0.0.2    
19         17         3.3.3.3/32       0             Gi1/0      10.0.0.2    
20         18         10.0.0.12/30     0             Gi1/0      10.0.0.2    
21         19         4.4.4.4/32       0             Gi1/0      10.0.0.2    
22         20         10.0.0.16/30     0             Gi1/0      10.0.0.2    
23         21         5.5.5.5/32       0             Gi1/0      10.0.0.2    
24         22         20.0.0.0/24      0             Gi1/0      10.0.0.2    
25         23         10.0.0.20/30     0             Gi1/0      10.0.0.2    
26         24         9.9.9.9/32       0             Gi1/0      10.0.0.2    
27         25         7.7.7.7/32       0             Gi1/0      10.0.0.2    
28         26         6.6.6.6/32       0             Gi1/0      10.0.0.2    
29         27         10.0.0.36/30     0             Gi1/0      10.0.0.2    
30         28         10.0.0.32/30     0             Gi1/0      10.0.0.2    
31         29         10.0.0.24/30     0             Gi1/0      10.0.0.2    
32         30         10.10.10.10/32   0             Gi1/0      10.0.0.2    
33         31         8.8.8.8/32       0             Gi1/0      10.0.0.2    
34         32         30.0.0.0/24      0             Gi1/0      10.0.0.2    
35         33         10.0.0.40/30     0             Gi1/0      10.0.0.2    
36         34         10.0.0.28/30     0             Gi1/0      10.0.0.2    
37         35         192.168.100.0/24 0             Gi1/0      10.0.0.2    

--------------------------------------------------

=== show running-config ===
Building configuration...

Current configuration : 2314 bytes
!
! Last configuration change at 07:35:20 UTC Thu Apr 2 2026
upgrade fpd auto
version 15.3
service timestamps debug datetime msec
service timestamps log datetime msec
service password-encryption
!
hostname router1
!
boot-start-marker
boot-end-marker
!
aqm-register-fnf
!
enable secret 5 $1$BW67$F5oif8Dj6Mb9XNbb5.F9K.
!
no aaa new-model
no ip icmp rate-limit unreachable
!
!
!
!
!
!
no ip domain lookup
ip domain name test.local
ip cef
no ipv6 cef
!
multilink bundle-name authenticated
!
!
!
!
!
!
!
!
!
username admin privilege 15 secret 5 $1$XE5S$NmZ2YFicQ/aoYUPCPh9/d1
!
redundancy
!
!
ip tcp synwait-time 5
! 
!
!
!
!
!
!
!
!
!
interface Loopback0
 ip address 1.1.1.1 255.255.255.255
 ip ospf 1 area 1
!
interface FastEthernet0/0
 no ip address
 shutdown
 duplex half
!
interface GigabitEthernet1/0
 description link to R2
 ip address 10.0.0.1 255.255.255.252
 ip nat outside
 ip virtual-reassembly in
 ip ospf network point-to-point
 ip ospf 1 area 1
 negotiation auto
 mpls ip
!
interface GigabitEthernet2/0
 no ip address
 negotiation auto
!
interface GigabitEthernet2/0.10
 encapsulation dot1Q 10
 ip address 192.168.0.1 255.255.255.0
 ip helper-address 192.168.2.2
!
interface GigabitEthernet2/0.20
 encapsulation dot1Q 20
 ip address 192.168.1.1 255.255.255.0
 ip helper-address 192.168.2.2
 ip nat inside
 ip virtual-reassembly in
!
interface GigabitEthernet2/0.30
 encapsulation dot1Q 30
 ip address 192.168.2.1 255.255.255.252
 ip nat inside
 ip virtual-reassembly in
!
router ospf 1
 router-id 1.1.1.1
!
ip forward-protocol nd
no ip http server
no ip http secure-server
!
!
ip nat inside source list 1 interface GigabitEthernet1/0 overload
!
no cdp log mismatch duplex
!
!
mpls ldp router-id Loopback0 force
access-list 1 permit 192.168.1.0 0.0.0.255
access-list 1 permit 192.168.2.0 0.0.0.3
!
control-plane
!
!
mgcp behavior rsip-range tgcp-only
mgcp behavior comedia-role none
mgcp behavior comedia-check-media-src disable
mgcp behavior comedia-sdp-force disable
!
mgcp profile default
!
!
!
gatekeeper
 shutdown
!
!
line con 0
 exec-timeout 0 0
 privilege level 15
 logging synchronous
 login local
 stopbits 1
line aux 0
 exec-timeout 0 0
 privilege level 15
 logging synchronous
 stopbits 1
line vty 0 4
 login local
 transport input ssh
line vty 5 15
 login local
 transport input ssh
!
!
end


--------------------------------------------------

