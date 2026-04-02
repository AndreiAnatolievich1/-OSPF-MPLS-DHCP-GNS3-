 Информация с роутера router5
# Дата: 2026-04-02 13:58:50.653256
# IP: 10.0.0.18
--------------------------------------------------

=== show ip ospf neighbor ===

Neighbor ID     Pri   State           Dead Time   Address         Interface
4.4.4.4           0   FULL/  -        00:00:33    10.0.0.17       GigabitEthernet2/0

--------------------------------------------------

=== show mpls forwarding-table ===
Local      Outgoing   Prefix           Bytes Label   Outgoing   Next Hop    
Label      Label      or Tunnel Id     Switched      interface              
16         16         1.1.1.1/32       0             Gi2/0      10.0.0.17   
17         17         2.2.2.2/32       0             Gi2/0      10.0.0.17   
18         18         3.3.3.3/32       0             Gi2/0      10.0.0.17   
19         Pop Label  4.4.4.4/32       0             Gi2/0      10.0.0.17   
20         19         10.0.0.0/30      0             Gi2/0      10.0.0.17   
21         20         10.0.0.4/30      0             Gi2/0      10.0.0.17   
22         Pop Label  10.0.0.8/30      0             Gi2/0      10.0.0.17   
23         Pop Label  10.0.0.12/30     0             Gi2/0      10.0.0.17   
24         23         10.0.0.20/30     0             Gi2/0      10.0.0.17   
25         24         9.9.9.9/32       0             Gi2/0      10.0.0.17   
26         25         7.7.7.7/32       0             Gi2/0      10.0.0.17   
27         26         6.6.6.6/32       0             Gi2/0      10.0.0.17   
28         27         10.0.0.36/30     0             Gi2/0      10.0.0.17   
29         28         10.0.0.32/30     0             Gi2/0      10.0.0.17   
30         29         10.0.0.24/30     0             Gi2/0      10.0.0.17   
31         30         10.10.10.10/32   0             Gi2/0      10.0.0.17   
32         31         8.8.8.8/32       0             Gi2/0      10.0.0.17   
33         32         30.0.0.0/24      0             Gi2/0      10.0.0.17   
34         33         10.0.0.40/30     0             Gi2/0      10.0.0.17   
35         34         10.0.0.28/30     0             Gi2/0      10.0.0.17   
37         22         20.0.0.0/24      0             Gi2/0      10.0.0.17   

--------------------------------------------------

=== show running-config ===
Building configuration...

Current configuration : 1927 bytes
!
! Last configuration change at 10:26:39 UTC Thu Apr 2 2026
upgrade fpd auto
version 15.3
service timestamps debug datetime msec
service timestamps log datetime msec
service password-encryption
!
hostname router5
!
boot-start-marker
boot-end-marker
!
aqm-register-fnf
!
enable secret 5 $1$c25y$48bJt/RxaivqG41nRfjDa0
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
username admin privilege 15 secret 5 $1$TYD8$RRY4DCuDNuc.6SLxtFmte.
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
 ip address 5.5.5.5 255.255.255.255
 ip ospf 1 area 2
!
interface FastEthernet0/0
 no ip address
 shutdown
 duplex half
!
interface GigabitEthernet1/0
 ip address 192.168.100.1 255.255.255.0
 negotiation auto
!
interface GigabitEthernet2/0
 description link to R4
 ip address 10.0.0.18 255.255.255.252
 ip ospf network point-to-point
 ip ospf 1 area 2
 negotiation auto
 mpls ip
!
interface GigabitEthernet3/0
 no ip address
 shutdown
 negotiation auto
!
router ospf 1
 router-id 5.5.5.5
 redistribute connected subnets route-map name
!
ip forward-protocol nd
no ip http server
no ip http secure-server
!
!
!
no cdp log mismatch duplex
!
route-map name permit 10
 match ip address 1
!
!
mpls ldp router-id Loopback0 force
access-list 1 permit 192.168.100.0 0.0.0.255
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

