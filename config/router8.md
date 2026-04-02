Информация с роутера router8
# Дата: 2026-04-02 13:58:58.423009
# IP: 10.0.0.30
--------------------------------------------------

=== show ip ospf neighbor ===

Neighbor ID     Pri   State           Dead Time   Address         Interface
7.7.7.7           0   FULL/  -        00:00:36    10.0.0.29       GigabitEthernet1/0

--------------------------------------------------

=== show mpls forwarding-table ===
Local      Outgoing   Prefix           Bytes Label   Outgoing   Next Hop    
Label      Label      or Tunnel Id     Switched      interface              
16         17         6.6.6.6/32       0             Gi1/0      10.0.0.29   
17         Pop Label  7.7.7.7/32       0             Gi1/0      10.0.0.29   
18         16         9.9.9.9/32       0             Gi1/0      10.0.0.29   
19         Pop Label  10.0.0.24/30     0             Gi1/0      10.0.0.29   
20         Pop Label  10.0.0.32/30     0             Gi1/0      10.0.0.29   
21         18         10.0.0.36/30     0             Gi1/0      10.0.0.29   
22         21         10.0.0.40/30     0             Gi1/0      10.0.0.29   
23         19         10.10.10.10/32   0             Gi1/0      10.0.0.29   
24         20         30.0.0.0/24      0             Gi1/0      10.0.0.29   
25         24         10.0.0.20/30     0             Gi1/0      10.0.0.29   
26         25         4.4.4.4/32       0             Gi1/0      10.0.0.29   
27         26         3.3.3.3/32       0             Gi1/0      10.0.0.29   
28         27         2.2.2.2/32       0             Gi1/0      10.0.0.29   
29         28         10.0.0.12/30     0             Gi1/0      10.0.0.29   
30         29         10.0.0.8/30      0             Gi1/0      10.0.0.29   
31         30         10.0.0.4/30      0             Gi1/0      10.0.0.29   
32         31         5.5.5.5/32       0             Gi1/0      10.0.0.29   
33         32         1.1.1.1/32       0             Gi1/0      10.0.0.29   
34         33         10.0.0.16/30     0             Gi1/0      10.0.0.29   
35         34         10.0.0.0/30      0             Gi1/0      10.0.0.29   
37         23         192.168.100.0/24 0             Gi1/0      10.0.0.29   

--------------------------------------------------

=== show running-config ===
Building configuration...

Current configuration : 1937 bytes
!
! Last configuration change at 10:24:31 UTC Thu Apr 2 2026
upgrade fpd auto
version 15.3
service timestamps debug datetime msec
service timestamps log datetime msec
service password-encryption
!
hostname router8
!
boot-start-marker
boot-end-marker
!
aqm-register-fnf
!
enable secret 5 $1$HSoL$0fK89be5ZUFCqDgLzN50D1
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
username admin privilege 15 secret 5 $1$Oyl6$Qx1u6WTPmMdk9j/9oY3HZ1
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
 ip address 8.8.8.8 255.255.255.255
 ip ospf 2 area 1
!
interface FastEthernet0/0
 no ip address
 shutdown
 duplex half
!
interface GigabitEthernet1/0
 description link to R7
 ip address 10.0.0.30 255.255.255.252
 ip ospf network point-to-point
 ip ospf 2 area 1
 negotiation auto
 mpls ip
!
interface GigabitEthernet2/0
 ip address 20.0.0.1 255.255.255.0
 negotiation auto
!
interface GigabitEthernet3/0
 ip address 40.0.0.1 255.255.255.0
 negotiation auto
!
router ospf 2
 router-id 8.8.8.8
 redistribute connected subnets route-map static-ospf2
!
ip forward-protocol nd
no ip http server
no ip http secure-server
!
!
!
no cdp log mismatch duplex
!
route-map static-ospf2 permit 10
 match ip address 1
!
!
mpls ldp router-id Loopback0
access-list 1 permit 20.0.0.0 0.0.0.255
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