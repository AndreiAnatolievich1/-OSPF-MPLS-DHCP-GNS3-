# Информация с роутера router2
# Дата: 2026-04-02 13:58:43.807452
# IP: 10.0.0.2
--------------------------------------------------

=== show ip ospf neighbor ===

Neighbor ID     Pri   State           Dead Time   Address         Interface
4.4.4.4           0   FULL/  -        00:00:35    10.0.0.10       GigabitEthernet3/0
3.3.3.3           0   FULL/  -        00:00:34    10.0.0.6        GigabitEthernet2/0
1.1.1.1           0   FULL/  -        00:00:35    10.0.0.1        GigabitEthernet1/0

--------------------------------------------------

=== show mpls forwarding-table ===
Local      Outgoing   Prefix           Bytes Label   Outgoing   Next Hop    
Label      Label      or Tunnel Id     Switched      interface              
16         Pop Label  1.1.1.1/32       0             Gi1/0      10.0.0.1    
17         Pop Label  3.3.3.3/32       0             Gi2/0      10.0.0.6    
18         Pop Label  10.0.0.12/30     5938          Gi2/0      10.0.0.6    
           Pop Label  10.0.0.12/30     0             Gi3/0      10.0.0.10   
19         Pop Label  4.4.4.4/32       534           Gi3/0      10.0.0.10   
20         Pop Label  10.0.0.16/30     6080          Gi3/0      10.0.0.10   
21         21         5.5.5.5/32       0             Gi3/0      10.0.0.10   
22         23         20.0.0.0/24      0             Gi2/0      10.0.0.6    
23         Pop Label  10.0.0.20/30     5830          Gi2/0      10.0.0.6    
24         24         9.9.9.9/32       0             Gi2/0      10.0.0.6    
25         25         7.7.7.7/32       0             Gi2/0      10.0.0.6    
26         26         6.6.6.6/32       0             Gi2/0      10.0.0.6    
27         27         10.0.0.36/30     0             Gi2/0      10.0.0.6    
28         28         10.0.0.32/30     0             Gi2/0      10.0.0.6    
29         29         10.0.0.24/30     7250          Gi2/0      10.0.0.6    
30         30         10.10.10.10/32   0             Gi2/0      10.0.0.6    
31         31         8.8.8.8/32       0             Gi2/0      10.0.0.6    
32         32         30.0.0.0/24      510           Gi2/0      10.0.0.6    
33         33         10.0.0.40/30     14768         Gi2/0      10.0.0.6    
34         34         10.0.0.28/30     7290          Gi2/0      10.0.0.6    
35         35         192.168.100.0/24 0             Gi3/0      10.0.0.10   

--------------------------------------------------

=== show running-config ===
Building configuration...

Current configuration : 1962 bytes
!
! Last configuration change at 07:45:24 UTC Thu Apr 2 2026
upgrade fpd auto
version 15.3
service timestamps debug datetime msec
service timestamps log datetime msec
service password-encryption
!
hostname router2
!
boot-start-marker
boot-end-marker
!
aqm-register-fnf
!
enable secret 5 $1$6GUc$3E15/vZjA2tV1ZSnSB18O/
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
username admin privilege 15 secret 5 $1$Chcl$99Atw34eBGrz9jJL72Eka.
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
 ip address 2.2.2.2 255.255.255.255
 ip ospf 1 area 0
!
interface FastEthernet0/0
 no ip address
 shutdown
 duplex half
!
interface GigabitEthernet1/0
 description link to R1
 ip address 10.0.0.2 255.255.255.252
 ip ospf network point-to-point
 ip ospf 1 area 1
 negotiation auto
 mpls ip
!
interface GigabitEthernet2/0
 description link to R3
 ip address 10.0.0.5 255.255.255.252
 ip ospf network point-to-point
 ip ospf 1 area 0
 negotiation auto
 mpls ip
!
interface GigabitEthernet3/0
 description link to R4
 ip address 10.0.0.9 255.255.255.252
 ip ospf network point-to-point
 ip ospf 1 area 0
 negotiation auto
 mpls ip
!
router ospf 1
 router-id 2.2.2.2
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
