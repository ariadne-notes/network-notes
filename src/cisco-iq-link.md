# Cisco IQ Link

- AKA, The Collector.

Optional, but allows tools like SNMP to query the devices directly for their telemetry.

Telemetry examples:
- Software Versions
- Crypto being used for VPNs

Can also use Data Connectors to talk to other Managers, like On-Prem SD-WAN Manager, or On-Prem Catalyst Center.

## VM Requirements

10K devices

- 16 vCPU
- 28GB RAM
- 600 GB

## IPv4 and DNS Requirements

- a v4 address
- DNS A Record (for the VM)
- DNS PTR Record (for the IP the VM is using)

## External Network Connectivity Requirements

These must work and be reachable in DNS.

**US Market**

- us-west-2.iq.cisco.com
- ng.acs.agent.us.csco.cloud

**EMEA Markt**

- eu-central-1.iq.cisco.com
- ng.acs.agent.emea.csco.cloud

**APJC Market**

- ap-southeast-2.iq.cisco.com
- ng.acs.agent.apjc.csco.cloud

## Port Requirements

| Port | Protocol | Purpose                     |
|------|----------|-----------------------------|
| 22   | TCP      | Admin CLI and Cisco Support |
| 443  | TCP      | Cisco IQ Link UI and API    |
| 53   | UDP/TCP  | DNS                         |
| 123  | UDP      | NTP                         |
| 161  | UDP      | SNMP                        |

## Supported Hypervisors

- VMWare ESXi
- Microsoft Hyper-V Server
- Red Hat KVM

## Internal Network Requirements

The internal network needs at least v4 `/20`, 4096 IPv4 addresses.

OK candidates are:

- `10.255.240.0/20`
- `192.168.240.0/20`
- `172.31.240.0/20`

**This cannot overlap** with anything Cisco IQ Link needs to reach on the managed network.

## Data Connectors

- Intersight
- SD-WAN Manager
- Meraki Dashboard
- On-Prem Catalyst Center

## References

[Cisco IQ Link Getting Started Guide v1.1.0 - Cisco](https://www.cisco.com/c/en/us/support/docs/cx/iq-link/cx225764-cisco-iq-link-getting-started-guide.html#toc-hId-97928839)
