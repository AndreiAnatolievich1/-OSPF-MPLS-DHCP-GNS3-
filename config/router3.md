# Информация с роутера router3
# Дата: 2026-04-02 13:58:45.787656
# IP: 10.0.0.6
--------------------------------------------------

=== show ip ospf neighbor ===

Neighbor ID     Pri   State           Dead Time   Address         Interface
6.6.6.6           0   FULL/  -        00:00:39    10.0.0.22       GigabitEthernet3/0
4.4.4.4           0   FULL/  -        00:00:38    10.0.0.14       GigabitEthernet1/0
2.2.2.2           0   FULL/  -        00:00:38    10.0.0.5        GigabitEthernet2/0

--------------------------------------------------

=== show mpls forwarding-table ===
Local      Outgoing   Prefix           Bytes Label   Outgoing   Next Hop    
Label      Label      or Tunnel Id     Switched      interface              
16         16         1.1.1.1/32       0             Gi2/0      10.0.0.5    
17         Pop Label  2.2.2.2/32       0             Gi2/0      10.0.0.5    
18         Pop Label  10.0.0.0/30      86612         Gi2/0      10.0.0.5    
19         Pop Label  10.0.0.8/30      0             Gi2/0      10.0.0.5    
           Pop Label  10.0.0.8/30      0             Gi1/0      10.0.0.14   
20         Pop Label  4.4.4.4/32       0             Gi1/0      10.0.0.14   
21         Pop Label  10.0.0.16/30     570           Gi1/0      10.0.0.14   
22         21         5.5.5.5/32       780           Gi1/0      10.0.0.14   
23         35         20.0.0.0/24      1334          Gi3/0      10.0.0.22   
24         16         9.9.9.9/32       0             Gi3/0      10.0.0.22   
25         20         7.7.7.7/32       0             Gi3/0      10.0.0.22   
26         Pop Label  6.6.6.6/32       0             Gi3/0      10.0.0.22   
27         Pop Label  10.0.0.36/30     0             Gi3/0      10.0.0.22   
28         21         10.0.0.32/30     0             Gi3/0      10.0.0.22   
29         Pop Label  10.0.0.24/30     6692          Gi3/0      10.0.0.22   
30         17         10.10.10.10/32   0             Gi3/0      10.0.0.22   
31         23         8.8.8.8/32       0             Gi3/0      10.0.0.22   
32         19         30.0.0.0/24      510           Gi3/0      10.0.0.22   
33         18         10.0.0.40/30     14768         Gi3/0      10.0.0.22   
34         22         10.0.0.28/30     7290          Gi3/0      10.0.0.22   
35         35         192.168.100.0/24 0             Gi1/0      10.0.0.14   

--------------------------------------------------

=== show running-config ===
Building configuration...

Current configuration : 2038 bytes
!
! Last configuration change at 09:27:09 UTC Thu Apr 2 2026
upgrade fpd auto
version 15.3
service timestamps debug datetime msec
service timestamps log datetime msec
service password-encryption
!
hostname router3
!
boot-start-marker
boot-end-marker
!
aqm-register-fnf
!
enable secret 5 $1$bP64$BlusgbR3buTOehshuRad51
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
username admin privilege 15 secret 5 $1$AGHa$rYv5TuHdGp.Ls9.eTlsSi.
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
 ip address 3.3.3.3 255.255.255.255
 ip ospf 1 area 0
!
interface FastEthernet0/0
 no ip address
 shutdown
 duplex half
!
interface GigabitEthernet1/0
 description link to R4
 ip address 10.0.0.13 255.255.255.252
 ip ospf network point-to-point
 ip ospf 1 area 0
 negotiation auto
 mpls ip
!
interface GigabitEthernet2/0
 description link to R2
 ip address 10.0.0.6 255.255.255.252
 ip ospf network point-to-point
 ip ospf 1 area 0
 negotiation auto
 mpls ip
!
interface GigabitEthernet3/0
 description link to R6
 ip address 10.0.0.21 255.255.255.252
 ip ospf network point-to-point
 ip ospf 1 area 0
 negotiation auto
 mpls ip
!
interface GigabitEthernet4/0
 no ip address
 shutdown
 negotiation auto
!
router ospf 1
 router-id 3.3.3.3
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