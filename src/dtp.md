# DTP

DTP is a Cisco proprietary point-to-point protocol, for full-duplex switchlinks.

An older feature intended to automate parts of network setup, you could set one switch to `dynamic desireable` and it will form trunks automatically.


## Modes

**switchport mode dynamic auto**

- *Default*
- Become a trunk if the neighbor is a trunk
- Become a trunk if the neighbor is set to `desireable`

**switchport mode access**

- Still sends DTP frames
- Asks the neighbor to become an access port

**switchport mode trunk**

- Still sends DTP frames
- Asks the neighbor to become a trunk port

**switchport mode dynamic desirable**

- Become a trunk only if the neighbor can be convinced to become a trunk
- Works with `trunk`, `desirable` or `auto`

**switchport nonegotiate**

- Do not send or respond to DTP
- Only works with `access` or `trunk`

## Config

```console
sw-11# show run int g0/2
 switchport mode access
 negotiation auto
```

## Capture

This is a DTP Packet, sent from an access port in `access`.

It's currently connected to a PC.

```plain
Frame 4: Packet, 90 bytes on wire (720 bits), 90 bytes captured (720 bits)
ISL
IEEE 802.3 Ethernet 
Logical-Link Control
Dynamic Trunk Protocol:  (Operating/Administrative): Access/Auto (0x04) (Operating/Administrative): ISL/Negotiated (0x40): aa:bb:cc:00:24:10
    Version: 1
    Domain
        Type: Domain (0x0001)
        Length: 5
        Domain: 
    Trunk Status
        Type: Trunk Status (0x0002)
        Length: 5
        Value: Access/Auto (0x04)
            0... .... = Trunk Operating Status: Access (0x0)
            .... .100 = Trunk Administrative Status: Auto (0x4)
    Trunk Type
        Type: Trunk Type (0x0003)
        Length: 5
        Value: ISL/Negotiated (0x40)
            010. .... = Trunk Operating Type: ISL (0x2)
            .... .000 = Trunk Administrative Type: Negotiated (0x0)
    Sender ID
        Type: Sender ID (0x0004)
        Length: 10
        Sender ID: aa:bb:cc:00:24:10 (aa:bb:cc:00:24:10)
```

## References

[Dynamic Trunking Protocol - Wikipedia](https://en.wikipedia.org/wiki/Dynamic_Trunking_Protocol)

[VLAN Configuration Guide, Cisco IOS XE 17.15.x (Catalyst 9500 Switches) - Configuring VLAN Trunks Cisco Catalyst 9500 Series Switches - Cisco](https://www.cisco.com/c/en/us/td/docs/switches/lan/catalyst9500/software/release/17-15/configuration_guide/vlan/b_1715_vlan_9500_cg/configuring_vlan_trunks.html)

