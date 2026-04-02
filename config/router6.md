Информация с роутера router6
# Дата: 2026-04-02 13:58:53.028573
# IP: 10.0.0.22
--------------------------------------------------

=== show ip ospf neighbor ===

Neighbor ID     Pri   State           Dead Time   Address         Interface
3.3.3.3           0   FULL/  -        00:00:39    10.0.0.21       GigabitEthernet3/0
7.7.7.7           0   FULL/  -        00:00:32    10.0.0.26       GigabitEthernet2/0
9.9.9.9           0   FULL/  -        00:00:38    10.0.0.37       GigabitEthernet1/0

--------------------------------------------------

=== show mpls forwarding-table ===
Local      Outgoing   Prefix           Bytes Label   Outgoing   Next Hop    
Label      Label      or Tunnel Id     Switched      interface              
16         Pop Label  9.9.9.9/32       561           Gi1/0      10.0.0.37   
17         18         10.10.10.10/32   0             Gi1/0      10.0.0.37   
18         Pop Label  10.0.0.40/30     13602         Gi1/0      10.0.0.37   
19         19         30.0.0.0/24      510           Gi1/0      10.0.0.37   
20         Pop Label  7.7.7.7/32       575           Gi2/0      10.0.0.26   
21         Pop Label  10.0.0.32/30     0             Gi2/0      10.0.0.26   
           Pop Label  10.0.0.32/30     0             Gi1/0      10.0.0.37   
22         Pop Label  10.0.0.28/30     6744          Gi2/0      10.0.0.26   
23         22         8.8.8.8/32       0             Gi2/0      10.0.0.26   
24         35         192.168.100.0/24 0             Gi3/0      10.0.0.21   
25         20         4.4.4.4/32       0             Gi3/0      10.0.0.21   
26         Pop Label  3.3.3.3/32       0             Gi3/0      10.0.0.21   
27         17         2.2.2.2/32       0             Gi3/0      10.0.0.21   
28         Pop Label  10.0.0.12/30     0             Gi3/0      10.0.0.21   
29         19         10.0.0.8/30      0             Gi3/0      10.0.0.21   
30         Pop Label  10.0.0.4/30      0             Gi3/0      10.0.0.21   
31         22         5.5.5.5/32       780           Gi3/0      10.0.0.21   
32         16         1.1.1.1/32       0             Gi3/0      10.0.0.21   
33         21         10.0.0.16/30     590           Gi3/0      10.0.0.21   
34         18         10.0.0.0/30      72036         Gi3/0      10.0.0.21   
35         35         20.0.0.0/24      1334          Gi2/0      10.0.0.26   

--------------------------------------------------

=== show running-config ===
Building configuration...

Current configuration : 1965 bytes
!
! Last configuration change at 09:25:24 UTC Thu Apr 2 2026
upgrade fpd auto
version 15.3
service timestamps debug datetime msec
service timestamps log datetime msec
service password-encryption
!
hostname router6
!
boot-start-marker
boot-end-marker
!
aqm-register-fnf
!
enable secret 5 $1$Er9/$vJCpxZJU/PVwl4AlremB.0
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
username admin privilege 15 secret 5 $1$LV2m$N1I0xgnzHL/0ulrSIZn2D.
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
 ip address 6.6.6.6 255.255.255.255
 ip ospf 2 area 0
!
interface FastEthernet0/0
 no ip address
 shutdown
 duplex half
!
interface GigabitEthernet1/0
 description link to R9
 ip address 10.0.0.38 255.255.255.252
 ip ospf network point-to-point
 ip ospf 2 area 0
 negotiation auto
 mpls ip
!
interface GigabitEthernet2/0
 description link to R7
 ip address 10.0.0.25 255.255.255.252
 ip ospf network point-to-point
 ip ospf 2 area 0
 negotiation auto
 mpls ip
!
interface GigabitEthernet3/0
 description link to R3
 ip address 10.0.0.22 255.255.255.252
 ip ospf network point-to-point
 ip ospf 2 area 0
 negotiation auto
 mpls ip
!
router ospf 2
 router-id 6.6.6.6
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