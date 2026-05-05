# Config
```
interface GigabitEthernet0/0
 switchport access vlan 10
 switchport mode access
 switchport port-security maximum 2
 switchport port-security
 spanning-tree portfast edge
```

# Validation
```
switch# show mac address-table secure 
          Mac Address Table
-------------------------------------------

Vlan    Mac Address       Type        Ports
----    -----------       --------    -----
  10    5254.000d.6573    STATIC      Gi0/0
```

```
switch# show port-security 
Secure Port  MaxSecureAddr  CurrentAddr  SecurityViolation  Security Action
                (Count)       (Count)          (Count)
---------------------------------------------------------------------------
      Gi0/0              2            1                  0         Shutdown
---------------------------------------------------------------------------
Total Addresses in System (excluding one mac per port)     : 0
Max Addresses limit in System (excluding one mac per port) : 4096
```

```
switch # show port-security interface gigabitEthernet 0/0
Port Security              : Enabled
Port Status                : Secure-up
Violation Mode             : Shutdown
Aging Time                 : 0 mins
Aging Type                 : Absolute
SecureStatic Address Aging : Disabled
Maximum MAC Addresses      : 2
Total MAC Addresses        : 1
Configured MAC Addresses   : 0
Sticky MAC Addresses       : 0
Last Source Address:Vlan   : 5254.000d.6573:10
Security Violation Count   : 0
```

# References
[Cisco - Port Security - IOS-XE 17.14 on the C9300](https://www.cisco.com/c/en/us/td/docs/switches/lan/catalyst9300/software/release/17-14/configuration_guide/sec/b_1714_sec_9300_cg/port_security.pdf)