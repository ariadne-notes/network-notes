# NAT64

* Embeds an IPv4 request into a IPv6 packet.
* Requires a DNS64 server.
* Requires a NAT64 router.


```
ipv6 unicast-routing
!
! Define NAT64 Prefix
!
nat64 prefix stateful 64:ff9b::/96
!
! Create NAT64 pool
!
nat64 v4 pool DHCP_POOL64 10.0.0.100 10.0.0.200

!
! Create ACL for NAT64 Clients
!
ipv6 access-list ACL_NAT64_CLIENTS
  permit ipv6 2001:db8:1::/48 any
!
!
nat64 v6v4 list ACL_NAT64_CLIENTS pool DHCP_POOL64 overload
!
! v6 network
!
interface GigabitEthernet3
 ipv6 address 2001:db8:1::1/64
 nat64 enable
!
! v4 network
!
interface GigabitEthernet1
 ip address 10.0.0.1 255.255.255.0
 nat64 enable
 
# References
[Cisco - IP Addressing Configuration Guide, Cisco IOS XE 17.x](https://www.cisco.com/c/en/us/td/docs/routers/ios/config/17-x/ip-addressing/b-ip-addressing/m_iadnat-stateful-nat64.html)
