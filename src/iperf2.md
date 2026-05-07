# Unicast

I like this test design for a few reasons.

- Doesn't overtax CML.
- 5 pps, we can start to get a feel for data flows.
- We can test how fast route recoveries or switchovers are.

### Server
```
iperf --server --port 2000 --interval 5
```

### Client
```
iperf --port 2000 --client 10.0.100.100 --reverse --time 3600 --interval 5 --udp --bandwidth 5pps  --len 1000
```

# Multicast

### Source
```
iperf --server --udp --bind 239.10.10.10 --interval 1
```

### Receiver
```
iperf --client 239.10.10.10 --udp --time 3600 --interval 1 --bandwidth 1pps --ttl 15 --len 1000
```

----

v1.1 - Last edit 6-May-2025

This work is dedicated to the Public Domain via [CC0 1.0](https://creativecommons.org/publicdomain/zero/1.0/)