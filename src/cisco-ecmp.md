# Cisco ECMP

BGP defaults to 1 path.

OSPF defaults to 4 paths.

EIGRP defaults to 4 paths.

## ECMP Algorithms

**Original**

Legacy. Oldest exists on all the gear.

**Universal**

Default. Selects based on (src-ip, dst-ip)

**Tunnel**

Recommended for flows between hosts.

Selects based on (src-ip, dst-ip, src-port, dst-port)

```console
ip cef load-sharing algorithm
```

## References

[IP Switching: Cisco Express Forwarding - Configuring a Load-Balancing Scheme for Cisco Express Forwarding Traffic Support - Cisco](https://www.cisco.com/c/en/us/td/docs/ios/ipswitch/configuration/guide/convert/ips_cef/cef_load_balancng.html)