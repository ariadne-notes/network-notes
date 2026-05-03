- This is one of the unidirectional preventatives.
- This is only for switch-to-switch ports.

# Terms
* **ULD** Unidirectional Link. A failure where one side of a fiber pair is broken.

# Topology

- SW1 is a root bridge.
- SW2 is experiencing a ULD failure.
- SW2 will transition Port 1 to a RP.

```
┌────────────────────┐                            ┌─────────────────────┐
│     ┌─────────────┐│                            │ ┌────────────┐      │
│     │ Port ┌────┐ ││ BPDU ────►                 │ │┌────┐ Port │      │
│     │  1   │ TX ├─││─────────────── Fiber Cut ──│─│┤ RX │  1   │      │
│ SW1 │      └────┘ ││                            │ │└────┘      │ SW2  │
│     │  RP  ┌────┐ ││                            │ │┌────┐  DP  │      │
│     │      │ RX ├─││────────────────────────────│─│┤ TX │      │      │
│     │      └────┘ ││                ◄───── BPDU │ │└────┘      │      │
│     └─────────────┘│                            │ └────────────┘      │
└────────────────────┘                            └─────────────────────┘
```


# Loopguard
If a port was receiving BPBUs and suddenly it stops, don't change the STP,


**default**
```
spanning-tree loopguard default
```

**pert port**
```
interface 1
  spanning-tree guard loop
```