# 10G Ethernet

## Terms

**WAN PHY**

These can operate with SONET/SDH at STS-192c.

**GPON**

Gigabit Passive Optical Network. Passive because none of the intermediate gear is powered.

| Media Type     | Wavelength      | Medium        | Distance  | IEEE Spec    | Other Description                           |
|----------------|-----------------|---------------|-----------|--------------|---------------------------------------------|
| SFP+           |                 | Direct Attach | ≤ 10 m    | vendor       | Can be copper or fiber                      |
| 10GBASE-CX4    | —               | Twinax copper | ≤ 15 m    | 802.3ak      | Tend to be bulky                            |
| 10GBASE-T      | —               | CAT 6a UTP    | ≤ 100 m   | 802.3an      | High power consumption Gets Hot             |
| 10GBASE-T1     | —               | 1 pair        | ≤ 15 m    | 802.3ch      | Automotive                                  |
| 10GBASE-LRM    | 1310 nm         | MMF           | ≤ 220 m   | 802.3aq      | "Long Reach Multimode"                      |
| 10GBASE-SR     | 850 nm          | MMF           | ≤ 300 m   | 802.3ae      | Uses 64B/66B encoding                       |
| 10GBASE-SW     | 850 nm          | MMF           | ≤ 300 m   | 802.3ae      | WAN PHY                                     |
| 10GBASE-LX4    | ~1310 nm x4     | MMF           | ≤ 300 m   | 802.3ae      | CWDM Can be MMF or SMF                      |
| 10GBASE-LR     | 1310 nm         | SMF           | ≤ 10 km   | 802.3ae      | Distributed feedback laser                  |
| 10GBASE-LW     | 1310 nm         | SMF           | ≤ 10 km   | 802.3ae      | WAN PHY,                                    |
| 10GBASE-LX4    | ~1310 nm x4     | SMF           | ≤ 10 km   | 802.3ae      | CWDM Can be MMF or SMF                      |
| 10GBASE-PR     | 1270nm & 1577nm | SMF           | < 20 km   | 802.3av      | GPON                                        |
| 10GBASE-EW     | 1550 nm         | SMF           | ≤ 40 km   | 802.3ae      | WAN PHY                                     |
| 10GBASE-ER     | 1550 nm         | SMF           | ≤ 40 km   | 802.3ae      | Externally modulated laser for range        |
| 10GBASE-ZR     | 1550 nm         | SMF           | ≤ 80 km   | vendor       | Not specified by IEEE, may not interoperate |

## References

[10 Gigabit Ethernet - Wikipedia](https://en.wikipedia.org/wiki/10_Gigabit_Ethernet)

[Testing PAM4 Signaling - 10GBASE-T1 Automotive](https://standards.ieee.org/wp-content/uploads/import/documents/other/eipatd-presentations/2021/d2-06.pdf)

[Cisco 10GBASE SFP+ Modules Data Sheet - Cisco](https://www.cisco.com/c/en/us/products/collateral/interfaces-modules/transceiver-modules/data_sheet_c78-455693.html)

[Mastering External Modulation in Lasers](https://www.numberanalytics.com/blog/ultimate-guide-external-modulation-laser-technology)
