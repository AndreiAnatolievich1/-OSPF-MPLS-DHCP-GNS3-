 Информация с роутера router9
# Дата: 2026-04-02 13:59:01.388806
# IP: 10.0.0.42
--------------------------------------------------

=== show ip ospf neighbor ===

Neighbor ID     Pri   State           Dead Time   Address         Interface
7.7.7.7           0   FULL/  -        00:00:36    10.0.0.34       GigabitEthernet3/0
6.6.6.6           0   FULL/  -        00:00:33    10.0.0.38       GigabitEthernet1/0
10.10.10.10       0   FULL/  -        00:00:31    10.0.0.41       GigabitEthernet2/0

--------------------------------------------------

=== show mpls forwarding-table ===
Local      Outgoing   Prefix           Bytes Label   Outgoing   Next Hop    
Label      Label      or Tunnel Id     Switched      interface              
16         Pop Label  6.6.6.6/32       0             Gi1/0      10.0.0.38   
17         Pop Label  10.0.0.24/30     0             Gi3/0      10.0.0.34   
           Pop Label  10.0.0.24/30     0             Gi1/0      10.0.0.38   
18         Pop Label  10.10.10.10/32   0             Gi2/0      10.0.0.41   
19         Pop Label  30.0.0.0/24      490           Gi2/0      10.0.0.41   
20         Pop Label  7.7.7.7/32       0             Gi3/0      10.0.0.34   
21         22         8.8.8.8/32       0             Gi3/0      10.0.0.34   
22         Pop Label  10.0.0.28/30     0             Gi3/0      10.0.0.34   
23         24         192.168.100.0/24 0             Gi1/0      10.0.0.38   
24         Pop Label  10.0.0.20/30     0             Gi1/0      10.0.0.38   
25         25         4.4.4.4/32       0             Gi1/0      10.0.0.38   
26         26         3.3.3.3/32       0             Gi1/0      10.0.0.38   
27         27         2.2.2.2/32       0             Gi1/0      10.0.0.38   
28         28         10.0.0.12/30     0             Gi1/0      10.0.0.38   
29         29         10.0.0.8/30      0             Gi1/0      10.0.0.38   
30         30         10.0.0.4/30      0             Gi1/0      10.0.0.38   
31         31         5.5.5.5/32       0             Gi1/0      10.0.0.38   
32         32         1.1.1.1/32       0             Gi1/0      10.0.0.38   
33         33         10.0.0.16/30     0             Gi1/0      10.0.0.38   
34         34         10.0.0.0/30      18666         Gi1/0      10.0.0.38   
35         35         20.0.0.0/24      0             Gi3/0      10.0.0.34   

--------------------------------------------------

=== show running-config ===
Building configuration...

Current configuration : 1982 bytes
!
! Last configuration change at 09:11:56 UTC Thu Apr 2 2026
upgrade fpd auto
version 15.3
service timestamps debug datetime msec
service timestamps log datetime msec
service password-encryption
!
hostname router9
!
boot-start-marker
boot-end-marker
!
aqm-register-fnf
!
enable secret 5 $1$/wG/$h0yYPEfxCc5V5aHTZviOn.
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
username admin privilege 15 secret 5 $1$9.sD$pAQfKVoffxCfUqvthwhmr1
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
 ip address 9.9.9.9 255.255.255.255
 ip ospf 2 area 0
!
interface FastEthernet0/0
 no ip address
 shutdown
 duplex half
!
interface GigabitEthernet1/0
 description link to R6
 ip address 10.0.0.37 255.255.255.252
 ip ospf network point-to-point
 ip ospf 2 area 0
 negotiation auto
 mpls ip
!
interface GigabitEthernet2/0
 description link to R10
 ip address 10.0.0.42 255.255.255.252
 ip ospf network point-to-point
 ip ospf 2 area 2
 negotiation auto
 mpls ip
!
interface GigabitEthernet3/0
 description link to R7
 ip address 10.0.0.33 255.255.255.252
 ip ospf network point-to-point
 ip ospf 2 area 0
 negotiation auto
 mpls ip
!
router ospf 2
 router-id 9.9.9.9
!
router ospf 3
!
ip forward-protocol nd
no ip http server
no ip http secure-server
!
!
!
no cdp log mismatch duplex
!
!
mpls ldp router-id Loopback0 force
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