# Subnet with fingers

I just memorize these sequences, ungainly, but works.

Decimal masks - `128, 192, 224, 240, 248, 252, 254, 255`

Wildcard masks - `127, 63, 31, 15, 7, 3, 1, 0`


# RFC 791 - Classful Networking

Early Internet addressing (1980s) the IP itself indicated the subnet mask, by using the High Order bits. There were only three network sizes.

/8 - Address starts with `0-127` - 128 networks

/16 - Address starts with `128-191` - 65,536 networks

/24 - Address starts with `192-223` - 16,777,216 networks

In the long ago, the hope was to use the first few bits of an address to tell the subnet mask. Even though we never do this in the modern era a few parts of classful networking are still here.

- `/24` is a very popular prefix
- `/16` is a very popular prefix
- All multicast addresses start with `1110`

```
Internet Protocol
Specification

  Addressing

    To provide for flexibility in assigning address to networks and
    allow for the  large number of small to intermediate sized networks
    the interpretation of the address field is coded to specify a small
    number of networks with a large number of host, a moderate number of
    networks with a moderate number of hosts, and a large number of
    networks with a small number of hosts.  In addition there is an
    escape code for extended addressing mode.

    Address Formats:

      High Order Bits   Format                           Class
      ---------------   -------------------------------  -----
            0            7 bits of net, 24 bits of host    a
            10          14 bits of net, 16 bits of host    b
            110         21 bits of net,  8 bits of host    c
            111         escape to extended addressing mode
```

# RFC1918 Dungeons

These are the most famous IPv4 networks. 

```
RFC 1918        Address Allocation for Private Internets   February 1996

3. Private Address Space

   The Internet Assigned Numbers Authority (IANA) has reserved the
   following three blocks of the IP address space for private internets:

     10.0.0.0        -   10.255.255.255  (10/8 prefix)
     172.16.0.0      -   172.31.255.255  (172.16/12 prefix)
     192.168.0.0     -   192.168.255.255 (192.168/16 prefix)

   We will refer to the first block as "24-bit block", the second as
   "20-bit block", and to the third as "16-bit" block. Note that (in
   pre-CIDR notation) the first block is nothing but a single class A
   network number, while the second block is a set of 16 contiguous
   class B network numbers, and third block is a set of 256 contiguous
   class C network numbers.
```
