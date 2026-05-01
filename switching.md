## Terms

* **Bridge ID:** 8 bytes. Bridge Priority + Extension ID (the vlan) + mac address.

**Example**
32768 / 1 / 52:54:00:4b:99:08

* **BPDU:** Bridge Protocol Data Unit. The frame used in 802.1D STP.

* **STP:** Spanning tree protocol. Frequently cited at 802.1D.

* **802.1D:** An IEEE standard. The oldest Ethernet STP.

* **Bridge:** A device that participates in the spanning tree algorithm. 

* **Designated ports:** send BPDUs downstream.

* **Root Port** Receive BPDUs. are the best port towards the root bridge, either the lowest total cost or the lowest advertised priority or lowest advertised port ID (interface number).


# Topology Algorithm

1. All bridges send BPDUs, as root.
1. All bridges compare BDPUs.
1. Bridge with lowest BPDU priority is root.
1. All ports on root bridge are DP, and BPDU cost field is set to zero.
1. DPs only sends BDPUs.
1. RPs only receive BPDUs.
1. A non-root bridge can only have one RP.
1. Root bridge sends BPDU (Root Identifier: Me, Bridge Identifier: Me, Cost: 0)
1. Non-root bridge gets two BPDUs, from two ports. It uses the Root Port Algo to select one port as RP.
1. Non-root bridge has RP. Sees cost in received BPDU, adds it's listed cost from the port it was received on.
1. Cost to root bridge is cost-in-BPDU + port cost. Copy that cost into the BPDUs to send out DPs.
1. Non-root bridge transitions all other ports to DP.
1. If a DP gets a BDPU, it means there is a loop, set this port to block.



# Port Selection Algo

- Lowest cost to root.
  - Lowest system priority of advertising switch.
    - Lowest MAC of advertising switch.
      - Lowest port priority on receiving switch.
        - Lowest interface number on receiving switch.

# Timers

* **Hello Time** is usually 2 seconds between BPDUs.
* **Forward Delay** is typically 15 seconds. It's between off -> listening -> learning.


## Root bridges election in Spanning Tree.

Two bridges send each other BPDUs, they compare bridge IDs to see who will keep sending BPDUs

The bridge with the lower ID (priority + mac address) wins. The non-root-bridge copies this bridge ID into it's BPDU, and sends that downstream.

The default for priority is `32768` or `0x80` on the wire. Because the 802.1D committee exists, the priority is this, plus the vlan ID.

**Always** configure a root bridge, or the *oldest device* with probably the *lowest mac address* wins the root bridge election.

```
switch(config)# spanning-tree vlan 60 priority ?
% Bridge Priority must be in increments of 4096.
% Allowed values are: 
  0     4096  8192  12288 16384 20480 24576 28672
  32768 36864 40960 45056 49152 53248 57344 61440
```

## Path Cost

The root bridge BPDU gets stuff tack'd onto it. The root bridge advertises itself as `0` cost.

Cost is the value of the link, towards the root bridge.

```
 ┌───────┐                                                                    
 │  SW1  │                                                                    
 └───┬───┘                                                                    
     │                                                                        
     │                                                                        
     │  Cost in BPDU from SW1 is 0                                                     
     │                                                                        
Eth0 │ ◄──── Interface is Assigned a cost of 100 by SW2 based on link Speed
 ┌───┴───┐                                                                    
 │  SW2  │                                                                    
 └───┬───┘                                                                    
Eth1 │                                                                        
     │                                                                        
     │   Cost in BPDU on-the-wire is now 100, SW2 Eth0 Cost                   
     │                                                                        
Eth0 │                                                                        
 ┌───┴───┐                                                                    
 │  SW2  │                                                                    
 └───────┘                                                                    
```


## Portfast
For end Hosts

* Does not protect against BPDUs

## Loop Prevention

Best practice is to set the root to `0` and the secondary to `4096`.



### STP Loop Guard

A unidirectional failure on a `root` or `alternate` port will cause spanning tree to loop, as other switches will unblock ports, and the unidirectional failure will still forward frames. To prevent this, turn on `stp loop guard` so ... if a port doesn't get a BPDU, it enters `STP loop-inconsistent` disabling the port.

This is done per interface, and is pretty tedious.

```
switch(config)# interface Ethernet 1/1
switch(config-if)# spanning-tree guard loop
```

More details [here](https://www.cisco.com/c/en/us/support/docs/lan-switching/spanning-tree-protocol/10596-84.html).

## Port Types

* **Designated ports:** send BPDUs downstream.

* **Root Ports** are the best port towards the root bridge, either the lowest total cost or the lowest advertised priority or lowest advertised port ID (interface number).

## BPDU Fields

**Root ID** - The bridge that has won and is winning the elections

**Bridge ID** - The bridge sending the BPDUs.

## Root Path Cost
**Root Path Cost** - What the interfaces costs + the advertised cost to the root. The root sends a cost of 0.

### STP Path Calculations
`spanning-tree pathcost method long`

| Speed      | Short-Mode Cost | Long-Mode Cost |
|------------|-----------------|----------------|
| 10 Mbps    | 100             | 2000000        |
| 100 Mbps   | 19              | 200000         |
| 1 Gbps     | 4               | 20000          |
| 10 Gbps    | 2               | 2000           |
| 20 Gbps    | 1               | 1000           |
| 40 Gbps    | 1               | 500            |
| 100 Gbps   | 1               | 200            |
| 1 Tbps     | 1               | 20             |
| 10 Tbps    | 1               | 2              |

## 802.1D - Spanning Tree

The 802.1D committee wanted *two* learning states[^stp], one with and one without learning station addresses. This is why it's more complicated.

[^stp]: *Interconnections* - Radia Perlman, page 67.

```
┌─────────────┐
│     off     │
└──────┬──────┘
       │
       │  Turn on interface
       │
┌──────▼──────┐
│  Listening  │ Receive + Send BPDUs
└──────┬──────┘
       │
       │  forward delay (default 15s)
       │
┌──────▼──────┐
│  Learning   │ Receive + Send BPDUs + Program CAM
└──────┬──────┘
       │
       │  forward delay (default 15s)
       │
┌──────▼──────┐
│  Forwarding │ Receive + Send BPDUs + Program CAM + Forward Frames
└─────────────┘
```

#### BPDU Frame Format

This is a RSTP BPDU.
```
Spanning Tree Protocol

    Protocol Identifier: Spanning Tree Protocol (0x0000)
    Protocol Version Identifier: Rapid Spanning Tree (2)
    BPDU Type: Rapid/Multiple Spanning Tree (2x02)
    BPDU flags: 0x3c, Forwarding, Learning, Port Role: Designated
    
    0... .... = Topology Change Acknowledgment: No
    .0.. .... = Agreement: No
    ..1. .... = Forwarding: Yes
    ...1 .... = Learning: Yes
    .... 11.. = Port Role: Designated (3)
    .... ..0. = Proposal: No
    .... ...0 = Topology Change: No
    
    Root Identifier: 32768 / 1 / aa:bb:cc:00:07:00
    Root Path Cost: 100
    
    Bridge Identifier: 32768 / 1 / aa:bb:cc:00:0a:00
    Port identifier: 0x8003
    Message Age: 1
    Max Age: 20
    Hello Time: 2
    Forward Delay: 15
    Version 1 Length: 0
```

This is what the BPDU looks like on-the-wire

```
┌───────────────────────────────┬───────────────┬───────────────┐
│                               │               │               │
│          Protocol ID          │    Version    │   BPDU Type   │
│                               │               │               │
│1 2 3 4 5 6 7 8 1 2 3 4 5 6 7 8│1 2 3 4 5 6 7 8│1 2 3 4 5 6 7 8│
└───────────────────────────────┴───────────────┴───────────────┘
             2 bytes                  1 byte         1 byte

┌───────────────┬───────────────────────────────────────────────►
│               │
│     Flag      │                    Root ID
│               │
│1 2 3 4 5 6 7 8│1 2 3 4 5 6 7 8 1 2 3 4 5 6 7 8 1 2 3 4 5 6 7 8
└───────────────┴───────────────────────────────────────────────►
    1 byte                            8 bytes

◄───────────────────────────────────────────────────────────────►

                           Root ID

 1 2 3 4 5 6 7 8 1 2 3 4 5 6 7 8 1 2 3 4 5 6 7 8 1 2 3 4 5 6 7 8
◄───────────────────────────────────────────────────────────────►
                          8 bytes

◄───────────────┬───────────────────────────────────────────────►
                │
    Root ID     │              Root Path Cost
                │
 1 2 3 4 5 6 7 8│1 2 3 4 5 6 7 8 1 2 3 4 5 6 7 8 1 2 3 4 5 6 7 8
◄───────────────┴───────────────────────────────────────────────►
    8 bytes                       4 bytes

◄───────────────┬───────────────────────────────────────────────►
 Root Path Cost │
                │                Bridge ID
                │
 1 2 3 4 5 6 7 8│1 2 3 4 5 6 7 8 1 2 3 4 5 6 7 8 1 2 3 4 5 6 7 8
◄───────────────┴───────────────────────────────────────────────►
  4 bytes                         8 bytes

◄───────────────────────────────────────────────────────────────►

                           Bridge ID

 1 2 3 4 5 6 7 8 1 2 3 4 5 6 7 8 1 2 3 4 5 6 7 8 1 2 3 4 5 6 7 8
◄───────────────────────────────────────────────────────────────►
                          8 bytes

◄───────────────┬───────────────────────────────┬───────────────►
                │                               │ Message age
   Bridge ID    │           Port ID             │  (in 1/256s of a second)
                │                               │
 1 2 3 4 5 6 7 8│1 2 3 4 5 6 7 8 1 2 3 4 5 6 7 8│1 2 3 4 5 6 7 8
◄───────────────┴───────────────────────────────┴───────────────►
    8 bytes                2 Bytes                   2 Bytes

◄───────────────┬───────────────────────────────┬───────────────►
                │           Max Age             │ Hello Time
   Message Age  │        (in 1/256ths)          │  (in 1/256ths of a second)
                │                               │
 1 2 3 4 5 6 7 8│1 2 3 4 5 6 7 8 1 2 3 4 5 6 7 8│1 2 3 4 5 6 7 8
◄───────────────┴───────────────────────────────┴───────────────►
    2 Bytes                 2 Bytes                   2 Bytes

◄───────────────┬───────────────────────────────┬───────────────┐
                │  Forward Delay                │   Version 1   │
   Hello Time   │    (in 1/256ths of a second)  │    Length     │
                │                               │               │
 1 2 3 4 5 6 7 8│1 2 3 4 5 6 7 8 1 2 3 4 5 6 7 8│1 2 3 4 5 6 7 8│
◄───────────────┴───────────────────────────────┴───────────────┘
    2 Bytes                 2 Bytes                   1 Byte

┌───────────────────────────────┐
│                               │
│      Version 3 Length         │
│                               │
│1 2 3 4 5 6 7 8 1 2 3 4 5 6 7 8│
└───────────────────────────────┘
           2 Bytes
```


## Topology Change Notifications (TCNs)

The default for Cisco is keeping a mac-address in CAM for 300 seconds.

Receiving a TCN sets this `max age` to the `forward delay` usually 15 seconds.

```
switch# show mac address-table aging-time 
Global Aging Time:  300
```

##### Finding TCNs
```
switch# show spanning-tree vlan 20 detail | s Spanning
 VLAN0020 is executing the rstp compatible Spanning Tree protocol
  Bridge Identifier has priority 32768, sysid 20, address aabb.cc00.0100
  Configured hello time 2, max age 20, forward delay 15, transmit hold-count 6
  Current root has priority 8212, address aabb.cc00.0200
  Root port is 7 (Ethernet1/2), cost of root path is 200
  Topology change flag not set, detected flag not set
  Number of topology changes 8 last change occurred 01:07:20 ago   < ----
          from Ethernet1/2                                         < ----
  Times:  hold 1, topology change 35, notification 2
          hello 2, max age 20, forward delay 15 
  Timers: hello 0, topology change 0, notification 0, aging 300
```

##### On the device

```
switch# show spanning-tree vlan 20 detail | i VLAN|transitions 
 VLAN0020 is executing the rstp compatible Spanning Tree protocol
 Port 2 (Ethernet0/1) of VLAN0020 is designated forwarding 
   Number of transitions to forwarding state: 2
 Port 4 (Ethernet0/3) of VLAN0020 is alternate blocking 
   Number of transitions to forwarding state: 1
 Port 7 (Ethernet1/2) of VLAN0020 is root forwarding 
   Number of transitions to forwarding state: 2
 Port 8 (Ethernet1/3) of VLAN0020 is alternate blocking 
   Number of transitions to forwarding state: 0
 Port 12 (Ethernet2/3) of VLAN0020 is designated forwarding 
   Number of transitions to forwarding state: 2
```

##### In the logs

```
switch# show logging | i %LINK
*Jul  8 04:22:24.660: %LINK-3-UPDOWN: Interface Ethernet0/0, changed state to up
*Jul  8 04:22:24.702: %LINK-3-UPDOWN: Interface Ethernet0/1, changed state to up
*Jul  8 04:22:24.715: %LINK-3-UPDOWN: Interface Ethernet0/2, changed state to up
*Jul  8 04:22:24.740: %LINK-3-UPDOWN: Interface Ethernet0/3, changed state to up
*Jul  8 04:22:24.769: %LINK-3-UPDOWN: Interface Ethernet1/0, changed state to up
*Jul  8 04:22:24.794: %LINK-3-UPDOWN: Interface Ethernet1/1, changed state to up
*Jul  8 04:22:24.819: %LINK-3-UPDOWN: Interface Ethernet1/2, changed state to up
*Jul  8 04:22:24.858: %LINK-3-UPDOWN: Interface Ethernet1/3, changed state to up
*Jul  8 04:22:24.888: %LINK-3-UPDOWN: Interface Ethernet2/0, changed state to up
*Jul  8 04:22:24.903: %LINK-3-UPDOWN: Interface Ethernet2/1, changed state to up
*Jul  8 04:22:24.927: %LINK-3-UPDOWN: Interface Ethernet2/2, changed state to up
*Jul  8 04:22:24.942: %LINK-3-UPDOWN: Interface Ethernet2/3, changed state to up
*Jul  8 04:22:24.965: %LINK-3-UPDOWN: Interface Ethernet3/0, changed state to up
*Jul  8 04:22:24.989: %LINK-3-UPDOWN: Interface Ethernet3/1, changed state to up
*Jul  8 04:22:25.013: %LINK-3-UPDOWN: Interface Ethernet3/2, changed state to up
*Jul  8 04:22:25.033: %LINK-3-UPDOWN: Interface Ethernet3/3, changed state to up
*Jul  8 04:22:26.685: %LINK-5-CHANGED: Interface Vlan1, changed state to administratively down
*Jul  8 04:24:58.575: %LINK-5-CHANGED: Interface Ethernet0/0, changed state to administratively down
*Jul  8 04:25:06.138: %LINK-3-UPDOWN: Interface Ethernet0/0, changed state to up
*Jul  8 04:26:59.260: %LINK-5-CHANGED: Interface Ethernet0/0, changed state to administratively down
*Jul  8 04:27:11.982: %LINK-3-UPDOWN: Interface Ethernet0/0, changed state to up
*Jul  8 04:28:43.205: %LINK-5-CHANGED: Interface Ethernet0/0, changed state to administratively down
*Jul  8 04:31:09.988: %LINK-3-UPDOWN: Interface Ethernet0/0, changed state to up
*Jul  8 04:33:53.881: %LINK-5-CHANGED: Interface Ethernet0/0, changed state to administratively down
*Jul  8 04:34:02.140: %LINK-3-UPDOWN: Interface Ethernet0/0, changed state to up
*Jul  8 05:00:52.111: %LINK-5-CHANGED: Interface Ethernet0/0, changed state to administratively down
*Jul  8 05:00:59.749: %LINK-3-UPDOWN: Interface Ethernet0/0, changed state to up
*Jul  8 05:03:48.728: %LINK-5-CHANGED: Interface Ethernet0/0, changed state to administratively down
*Jul  8 05:03:54.050: %LINK-3-UPDOWN: Interface Ethernet0/0, changed state to up
*Jul  8 05:07:04.113: %LINK-5-CHANGED: Interface Ethernet0/0, changed state to administratively down
*Jul  8 05:07:06.713: %LINK-3-UPDOWN: Interface Ethernet0/0, changed state to up
*Jul  8 05:07:31.603: %LINK-5-CHANGED: Interface Ethernet0/0, changed state to administratively down
*Jul  8 05:07:36.280: %LINK-3-UPDOWN: Interface Ethernet0/0, changed state to up
*Jul  8 05:11:32.247: %LINK-3-UPDOWN: Interface Vlan10, changed state to up
*Jul  8 06:35:29.308: %LINK-5-CHANGED: Interface Ethernet0/0, changed state to administratively down
*Jul  8 06:35:43.756: %LINK-3-UPDOWN: Interface Ethernet0/0, changed state to up
```
