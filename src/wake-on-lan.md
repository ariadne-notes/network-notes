# Wake on LAN

AKA, *Magic Packets*

- Usually a UDP broadcast `255.255.255.255` frame with `FF FF FF FF FF FF` as it's payload
  - Typically UDP ports 7 or UDP port 9
- 16 repetitions of the target computers 48-bit MAC Address
- Sometimes sent as a directed broadcast `10.0.0.1/24` becomes `10.0.0.255`
  - Directed broadcasts require the routers be configured to allow them
  
```plain
Frame 1: Packet, 116 bytes on wire (928 bits), 116 bytes captured (928 bits)
Ethernet II, Src: Intel_85:cf:01 (00:90:27:85:cf:01), Dst: Broadcast (ff:ff:ff:ff:ff:ff)
Wake On LAN, MAC: Dell_dc:9e:35 (00:0d:56:dc:9e:35)
    Sync stream: ffffffffffff
    MAC: Dell_dc:9e:35 (00:0d:56:dc:9e:35)
        MAC: Dell_dc:9e:35 (00:0d:56:dc:9e:35)
        MAC: Dell_dc:9e:35 (00:0d:56:dc:9e:35)
        MAC: Dell_dc:9e:35 (00:0d:56:dc:9e:35)
        MAC: Dell_dc:9e:35 (00:0d:56:dc:9e:35)
        MAC: Dell_dc:9e:35 (00:0d:56:dc:9e:35)
        MAC: Dell_dc:9e:35 (00:0d:56:dc:9e:35)
        MAC: Dell_dc:9e:35 (00:0d:56:dc:9e:35)
        MAC: Dell_dc:9e:35 (00:0d:56:dc:9e:35)
        MAC: Dell_dc:9e:35 (00:0d:56:dc:9e:35)
        MAC: Dell_dc:9e:35 (00:0d:56:dc:9e:35)
        MAC: Dell_dc:9e:35 (00:0d:56:dc:9e:35)
        MAC: Dell_dc:9e:35 (00:0d:56:dc:9e:35)
        MAC: Dell_dc:9e:35 (00:0d:56:dc:9e:35)
        MAC: Dell_dc:9e:35 (00:0d:56:dc:9e:35)
        MAC: Dell_dc:9e:35 (00:0d:56:dc:9e:35)
        MAC: Dell_dc:9e:35 (00:0d:56:dc:9e:35)
```

Packet courtesy of the [Wiki Wireshark](https://en.wikipedia.org/wiki/Wake-on-LAN)

## References

[Wake-on-LAN - Wikipedia](https://en.wikipedia.org/wiki/Wake-on-LAN)
