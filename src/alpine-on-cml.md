# Alpine Hosts
```
USERNAME=cisco
PASSWORD=cisco
hostname pc-20
ip link set dev eth0 up
ip address add 10.0.20.20/24 dev eth0
ip route add default via 10.0.20.1
```