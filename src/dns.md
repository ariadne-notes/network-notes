DNS uses TCP and UDP.

* UDP, for user queries
* TCP, to do zone transfers (how DNS replicates it's records to other DNS boxes)

## DNS Resource Records

| RR    | Description                                                           |
| ----- | --------------------------------------------------------------------- |
| A     | v4 IP Address                                                         |
| AAAA  | v6 IP Address                                                         |
| CNAME | Alias or nickname. Secondary Name                                     |
| MX    | Email server                                                          |
| NS    | DNS Server                                                            |
| PTR   | Reverse Mapping of an IP. Used to find the host that "owns" the IP    |
| SOA   | Start of Authority. Which DNS server is authorative for the zone.     |