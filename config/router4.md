 Информация с роутера router4
# Дата: 2026-04-02 13:58:48.163478
# IP: 10.0.0.14
--------------------------------------------------

=== show ip ospf neighbor ===

Neighbor ID     Pri   State           Dead Time   Address         Interface
3.3.3.3           0   FULL/  -        00:00:39    10.0.0.13       GigabitEthernet1/0
2.2.2.2           0   FULL/  -        00:00:38    10.0.0.9        GigabitEthernet3/0
5.5.5.5           0   FULL/  -        00:00:37    10.0.0.18       GigabitEthernet2/0

--------------------------------------------------

=== show mpls forwarding-table ===
Local      Outgoing   Prefix           Bytes Label   Outgoing   Next Hop    
Label      Label      or Tunnel Id     Switched      interface              
16         16         1.1.1.1/32       0             Gi3/0      10.0.0.9    
17         Pop Label  2.2.2.2/32       0             Gi3/0      10.0.0.9    
18         Pop Label  3.3.3.3/32       0             Gi1/0      10.0.0.13   
19         Pop Label  10.0.0.0/30      16958         Gi3/0      10.0.0.9    
20         Pop Label  10.0.0.4/30      0             Gi3/0      10.0.0.9    
           Pop Label  10.0.0.4/30      0             Gi1/0      10.0.0.13   
21         Pop Label  5.5.5.5/32       702           Gi2/0      10.0.0.18   
22         23         20.0.0.0/24      1334          Gi1/0      10.0.0.13   
23         Pop Label  10.0.0.20/30     0             Gi1/0      10.0.0.13   
24         24         9.9.9.9/32       0             Gi1/0      10.0.0.13   
25         25         7.7.7.7/32       0             Gi1/0      10.0.0.13   
26         26         6.6.6.6/32       0             Gi1/0      10.0.0.13   
27         27         10.0.0.36/30     0             Gi1/0      10.0.0.13   
28         28         10.0.0.32/30     0             Gi1/0      10.0.0.13   
29         29         10.0.0.24/30     0             Gi1/0      10.0.0.13   
30         30         10.10.10.10/32   0             Gi1/0      10.0.0.13   
31         31         8.8.8.8/32       0             Gi1/0      10.0.0.13   
32         32         30.0.0.0/24      0             Gi1/0      10.0.0.13   
33         33         10.0.0.40/30     0             Gi1/0      10.0.0.13   
34         34         10.0.0.28/30     0             Gi1/0      10.0.0.13   
35         Pop Label  192.168.100.0/24 0             Gi2/0      10.0.0.18   

--------------------------------------------------

=== show running-config ===
Building configuration...

Current configuration : 1965 bytes
!
! Last configuration change at 07:58:53 UTC Thu Apr 2 2026
upgrade fpd auto
version 15.3
service timestamps debug datetime msec
service timestamps log datetime msec
service password-encryption
!
hostname router4
!
boot-start-marker
boot-end-marker
!
aqm-register-fnf
!
enable secret 5 $1$JSj.$./GhlPxj9k.69FDqsQApN0
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
username admin privilege 15 secret 5 $1$tMrp$LFd0PpU8SMEKdFH9Xt5uD/
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
 ip address 4.4.4.4 255.255.255.255
 ip ospf 1 area 0
!
interface FastEthernet0/0
 no ip address
 shutdown
 duplex half
!
interface GigabitEthernet1/0
 description link to R3
 ip address 10.0.0.14 255.255.255.252
 ip ospf network point-to-point
 ip ospf 1 area 0
 negotiation auto
 mpls ip
!
interface GigabitEthernet2/0
 description link to R5
 ip address 10.0.0.17 255.255.255.252
 ip ospf network point-to-point
 ip ospf 1 area 2
 negotiation auto
 mpls ip
!
interface GigabitEthernet3/0
 description link to R2
 ip address 10.0.0.10 255.255.255.252
 ip ospf network point-to-point
 ip ospf 1 area 0
 negotiation auto
 mpls ip
!
router ospf 1
 router-id 4.4.4.4
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