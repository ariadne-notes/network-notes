# ISIS

IS-IS
: Intermediate System To Intermediate System
  - An ISO standard open protocol.
  - Link State and Shortest Path
  - Good for large flat networks

IS
: Intermediate System
  - A router
  
ES
: End Station
  - A PC, or a server.

- Default route injected via route-map.

- **Station Routing**
  - AKA, intra-area
  - Routing within a L1 area.
  
- **Area Routing**
  - AKA, inter-area
  - Routing within a L2 area.
  
Backbone
  : The L2 area.


```console
router isis 
 net 00.0000.0000.0005.00
 interface g0/0/0/0
   address-family ipv4
```


ISIS Uses a simplified topology compared to OSPF.

There are only three kinds of routers.

- **L2** Backbone routers, which can carry summaries.

- **L1L2:** ABRs, AKA Area Border Routers.

- **L1:** These are the Area routers. They do not flood their link state databases into L2.

## Example


<pre>
                                             ┌──────┐
                                             │ L1   │
                                             └───┬──┘
                                                 │
┌──────┐     ┌──────┐        ┌──────┐        ┌───┴──┐     ┌─────┐
│  L1  ├─────┤ L1L2 ├────────┤  L2  ├────────┤ L1L2 ├─────┤ L1  │
└───┬──┘     └───┬──┘        └──────┘        └───┬──┘     └─────┘
    │            └──────┐                  ┌─────┘
┌───┴──┐             ┌──┴───┐          ┌───┴──┐
│  L1  │             │  L2  │──────────┤  L2  │
└──────┘             └──────┘          └──────┘
</pre>


## Topology

Single Topology
  : All Routed Protocols must be configured on all enabled interfaces.
  : e.g. v4 and v6 on all interfaces.
  
Multi-Topology
  : Some interfaces can be v4, others can be v6, others can be both.

### Addressing Scheme

This is the ISO NSAP addressing format.
- Minimum of 8 bytes.
- Max of 20 bytes

```plain
┌──────────────┬─────────────────────────────────────────┬──────┐
│ Area Address │                System ID                │ NSEL │
└──────────────┴─────────────────────────────────────────┴──────┘
    Variable                     6 bytes                  1 Byte 
```


net 00.0000.0000.0005.00

Area   - System ID - N-Selector
1 byte - 6 bytes   - 1 byte.
area is not the same
Does not denote flooding domain

N-Selector is always zero
System ID is only unique to the area.
Address is always an even number of bytes, 8 to 20.

49 is a private address.

So long as the NSAP is unique, its OK because we aren't routing CLNS.

Priority is used for the CLNS election.
Circuit ID, who won the election

Level 2 can be Inter or Intra area
Level 1 is Intra area only.
Level 2 is the backbone.

Level 1 (NSSA)
    - Intraarea
    - Default route out (sets the attached bit)
    - Redistribution is allowed.

L1/L2 is the ABR in OSPF

ISIS does not ride IP, it rides CLNS.
To do Multipoint NBMA you need to include CLNS resolution.

L1 areas must match


## IS-IS Metric Styles

IS-IS supports two metric styles relevant to MPLS-TE:

| Style       | Description |
|-------------|--------------------------------------------------------------|
| **Narrow**  | Legacy metric style; max metric value 63 per link, 1023 path |
| **Wide**    | Extended metric style; max metric of 16 772 215.             |

### Configuration

Enable wide metrics:

```console
metric-style wide
```

### Transition Commands

Used when migrating from narrow to wide without a hard cutover:

| Command                           | Behavior                                                 |
|-----------------------------------|----------------------------------------------------------|
| `metric-style transition`         | Advertises **both** narrow and wide TLVs simultaneously  |
| `metric-style narrow transition`  | Transitioning — still **advertising narrow** (old)       |
| `metric-style wide transition`    | Transitioning — now **advertising wide** (new)           |

# References

[RFC 1195: Use of OSI IS-IS for routing in TCP/IP and dual environments | RFC Editor](https://www.rfc-editor.org/info/rfc1195/)

[ISO/IEC 10589:2002 - Information technology — Telecommunications and information exchange between systems](https://www.iso.org/standard/30932.html)

[ISO/IEC 8348:2002 - Information technology — Open Systems Interconnection — Network service definition](https://www.iso.org/standard/35872.html)
