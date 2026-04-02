Информация с роутера router10
# Дата: 2026-04-02 13:59:04.378576
# IP: 10.0.0.41
--------------------------------------------------

=== show ip ospf neighbor ===

Neighbor ID     Pri   State           Dead Time   Address         Interface
9.9.9.9           0   FULL/  -        00:00:33    10.0.0.42       GigabitEthernet2/0

--------------------------------------------------

=== show mpls forwarding-table ===
Local      Outgoing   Prefix           Bytes Label   Outgoing   Next Hop    
Label      Label      or Tunnel Id     Switched      interface              
16         Pop Label  9.9.9.9/32       0             Gi2/0      10.0.0.42   
17         16         6.6.6.6/32       0             Gi2/0      10.0.0.42   
18         Pop Label  10.0.0.36/30     0             Gi2/0      10.0.0.42   
19         17         10.0.0.24/30     0             Gi2/0      10.0.0.42   
20         20         7.7.7.7/32       0             Gi2/0      10.0.0.42   
21         Pop Label  10.0.0.32/30     0             Gi2/0      10.0.0.42   
22         22         10.0.0.28/30     0             Gi2/0      10.0.0.42   
23         21         8.8.8.8/32       0             Gi2/0      10.0.0.42   
24         23         192.168.100.0/24 0             Gi2/0      10.0.0.42   
25         24         10.0.0.20/30     0             Gi2/0      10.0.0.42   
26         25         4.4.4.4/32       0             Gi2/0      10.0.0.42   
27         26         3.3.3.3/32       0             Gi2/0      10.0.0.42   
28         27         2.2.2.2/32       0             Gi2/0      10.0.0.42   
29         28         10.0.0.12/30     0             Gi2/0      10.0.0.42   
30         29         10.0.0.8/30      0             Gi2/0      10.0.0.42   
31         30         10.0.0.4/30      0             Gi2/0      10.0.0.42   
32         31         5.5.5.5/32       0             Gi2/0      10.0.0.42   
33         32         1.1.1.1/32       0             Gi2/0      10.0.0.42   
34         33         10.0.0.16/30     0             Gi2/0      10.0.0.42   
35         34         10.0.0.0/30      0             Gi2/0      10.0.0.42   
36         35         20.0.0.0/24      0             Gi2/0      10.0.0.42   

--------------------------------------------------

=== show running-config ===
Building configuration...

Current configuration : 1810 bytes
!
! Last configuration change at 08:53:34 UTC Thu Apr 2 2026
upgrade fpd auto
version 15.3
service timestamps debug datetime msec
service timestamps log datetime msec
service password-encryption
!
hostname router10
!
boot-start-marker
boot-end-marker
!
aqm-register-fnf
!
enable secret 5 $1$510D$jIaMdD/XsN2k.LNYNDTLG/
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
username admin privilege 15 secret 5 $1$UITg$p8EiDQGOa8DhgIbsG8eck0
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
 ip address 10.10.10.10 255.255.255.255
 ip ospf 2 area 2
!
interface FastEthernet0/0
 no ip address
 shutdown
 duplex half
!
interface GigabitEthernet1/0
 ip address 30.0.0.1 255.255.255.0
 ip ospf 2 area 2
 negotiation auto
!
interface GigabitEthernet2/0
 description link ro R9
 ip address 10.0.0.41 255.255.255.252
 ip ospf network point-to-point
 ip ospf 2 area 2
 negotiation auto
 mpls ip
!
interface GigabitEthernet3/0
 no ip address
 shutdown
 negotiation auto
!
router ospf 2
 router-id 10.10.10.10
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
