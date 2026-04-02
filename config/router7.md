 Информация с роутера router7
# Дата: 2026-04-02 13:58:55.576849
# IP: 10.0.0.26
--------------------------------------------------

=== show ip ospf neighbor ===

Neighbor ID     Pri   State           Dead Time   Address         Interface
9.9.9.9           0   FULL/  -        00:00:33    10.0.0.33       GigabitEthernet3/0
6.6.6.6           0   FULL/  -        00:00:37    10.0.0.25       GigabitEthernet2/0
8.8.8.8           0   FULL/  -        00:00:33    10.0.0.30       GigabitEthernet1/0

--------------------------------------------------

=== show mpls forwarding-table ===
Local      Outgoing   Prefix           Bytes Label   Outgoing   Next Hop    
Label      Label      or Tunnel Id     Switched      interface              
16         Pop Label  9.9.9.9/32       0             Gi3/0      10.0.0.33   
17         Pop Label  6.6.6.6/32       0             Gi2/0      10.0.0.25   
18         Pop Label  10.0.0.36/30     0             Gi2/0      10.0.0.25   
           Pop Label  10.0.0.36/30     0             Gi3/0      10.0.0.33   
19         18         10.10.10.10/32   0             Gi3/0      10.0.0.33   
20         19         30.0.0.0/24      0             Gi3/0      10.0.0.33   
21         Pop Label  10.0.0.40/30     0             Gi3/0      10.0.0.33   
22         Pop Label  8.8.8.8/32       0             Gi1/0      10.0.0.30   
23         24         192.168.100.0/24 0             Gi2/0      10.0.0.25   
24         Pop Label  10.0.0.20/30     0             Gi2/0      10.0.0.25   
25         25         4.4.4.4/32       0             Gi2/0      10.0.0.25   
26         26         3.3.3.3/32       0             Gi2/0      10.0.0.25   
27         27         2.2.2.2/32       0             Gi2/0      10.0.0.25   
28         28         10.0.0.12/30     0             Gi2/0      10.0.0.25   
29         29         10.0.0.8/30      0             Gi2/0      10.0.0.25   
30         30         10.0.0.4/30      0             Gi2/0      10.0.0.25   
31         31         5.5.5.5/32       780           Gi2/0      10.0.0.25   
32         32         1.1.1.1/32       0             Gi2/0      10.0.0.25   
33         33         10.0.0.16/30     590           Gi2/0      10.0.0.25   
34         34         10.0.0.0/30      18076         Gi2/0      10.0.0.25   
35         Pop Label  20.0.0.0/24      1266          Gi1/0      10.0.0.30   

--------------------------------------------------

=== show running-config ===
Building configuration...

Current configuration : 1965 bytes
!
! Last configuration change at 09:08:53 UTC Thu Apr 2 2026
upgrade fpd auto
version 15.3
service timestamps debug datetime msec
service timestamps log datetime msec
service password-encryption
!
hostname router7
!
boot-start-marker
boot-end-marker
!
aqm-register-fnf
!
enable secret 5 $1$oOqr$8GX1flYiKwJXBuZoSCOl40
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
username admin privilege 15 secret 5 $1$63I5$Mavo9PS45F0jahfeCfHhp0
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
 ip address 7.7.7.7 255.255.255.255
 ip ospf 2 area 0
!
interface FastEthernet0/0
 no ip address
 shutdown
 duplex half
!
interface GigabitEthernet1/0
 description link to R8
 ip address 10.0.0.29 255.255.255.252
 ip ospf network point-to-point
 ip ospf 2 area 1
 negotiation auto
 mpls ip
!
interface GigabitEthernet2/0
 description link to R6
 ip address 10.0.0.26 255.255.255.252
 ip ospf network point-to-point
 ip ospf 2 area 0
 negotiation auto
 mpls ip
!
interface GigabitEthernet3/0
 description link to R9
 ip address 10.0.0.34 255.255.255.252
 ip ospf network point-to-point
 ip ospf 2 area 0
 negotiation auto
 mpls ip
!
router ospf 2
 router-id 7.7.7.7
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