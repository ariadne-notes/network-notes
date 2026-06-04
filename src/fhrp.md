# FHRP

Useful for hosts, which cannot configure more than one IP.

FHRPs can suffer from asymmetrical routing, if two switches advertise the same subnet, upstream equipment cannot know which switch is the primary.

This is generally not a problem if both switches can still reach both hosts, but becomes a problem with spanning tree blocking ports.

## Terms

FHRP
: First Hop Routing Protocol

VIP
: The shared IP in a a first-hop routing protocol.

## References

[Network Services Configuration Guide, Cisco IOS XE 17.x - Configuring VRRP Cisco IOS XE 17 - Cisco](https://www.cisco.com/c/en/us/td/docs/routers/ios/config/17-x/ntw-servs/b-network-services/m_fhp-vrrp-0.html)

[RFC 9568: Virtual Router Redundancy Protocol (VRRP) Version 3 for IPv4 and IPv6 | RFC Editor](https://www.rfc-editor.org/info/rfc9568/)
