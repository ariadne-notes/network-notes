# FHRP

FHRP
: First Hop Routing Protocol

VIP
: The shared IP in a a first-hop routing protocol.

## GLBP

GLBP 
: Gateway Load Balancing Protocol
- Cisco Proprietary
- Supports 4 active AVFs
  
AVG
: Active Virtual Gateway
- The AVG is responsible for answering incoming ARP for the VIP.
- Can reply with a different MAC addresses to load balance.
- Highest priority AVF is the AVG.

AVF 
: Active Virtual Forwarder
- Two states {Active, Listen}
- A router in a GLBP group that is forwarding packets.
- All AVFs have their own mac and forwarding traffic destined towards that MAC.
- 4 max.

### Details

- 224.0.0.102
- UDP 3222

- MD5 is supported

## References

[Cisco - Configuring GLBP](https://www.cisco.com/c/en/us/td/docs/routers/ios/config/17-x/ntw-servs/b-network-services/m_fhp-glbp-0.pdf)
