# Portfast

The biggest reason to configure portfast, is portfast tells spanning-tree, this isn't a p2p port, so please don't send a TCN.
TCNs have all devices in the STP topoolgy refresh their mac-address tables.

## Immediately forward traffic

802.1D waits normally 30 seconds (2x the forward delay) before forwarding traffic. This means a modern computer, with Ethernet, will be powered on, without network, waiting those 30 seconds for DHCP to complete.

PXE (booting from network) ports should always be portfast.

## Enable on all access ports


```console
spanning-tree portfast default
```

## Enable on trunks

Some trunk ports connect to servers. Portfast can be enabled on those too.

```console
spanning-tree portfast trunk
```
