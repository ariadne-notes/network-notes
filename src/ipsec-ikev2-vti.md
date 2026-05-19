# IPSec IKEv2 VTI

```
hostname R1
!
crypto ikev2 keyring IKR_MY_KEYS
 peer R5
  address 10.0.45.5
  hostname R5
  identity address 10.0.45.5
  pre-shared-key Cisco!123
 !
 peer R1
  address 10.0.12.1
  hostname R1
  identity address 10.0.12.1
  pre-shared-key Cisco!123
 !
!
!
crypto ikev2 profile IV2P_TUNNELS
 match identity remote any
 authentication remote pre-share key IKR_MY_KEYS
 authentication local pre-share key IKR_MY_KEYS
!
crypto ipsec transform-set IPSEC_TRANSFORM_TUNNELS esp-aes 256 esp-sha512-hmac 
 mode tunnel
!
crypto ipsec profile IPSEC_PROFILE_TUNNELS
 set ikev2-profile IV2P_TUNNELS
!
interface Loopback0
 ip address 1.1.1.1 255.255.255.255
 ip ospf 1 area 0
!
interface Tunnel15
 ip address 10.0.15.1 255.255.255.0
 ip ospf 1 area 0
 tunnel source GigabitEthernet1
 tunnel mode ipsec ipv4
 tunnel destination 10.0.45.5
 tunnel protection ipsec profile IPSEC_PROFILE_TUNNELS
!
interface GigabitEthernet1
 ip address 10.0.12.1 255.255.255.0
 no shutdown
!
router ospf 1
 passive-interface GigabitEthernet1
 passive-interface Loopback0
!
line con 0
 transport preferred none
```