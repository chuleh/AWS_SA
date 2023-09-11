# Subnets Overview

* subrange of IPv4s in your VPC
* AWS reserves 5 IP addresses in each subnet (first 4 && last 1)
  * ex: if CIDR 10.0.0.0/24 => reserved ips are:
    * 10.0.0.0 - network address
    * 10.0.0.1 - AWS VPC Router
    * 10.0.0.2 - for mapping to Amazon-provided DNS
    * 10.0.0.3 - reserved for future use
    * 10.0.0.255 - Network Broadcast Address - no supported in VPC therefore reserved
* tip: if you need 32 IPs can't choose /27 == (32 ips - 5 (amazon reserved) == 27 < 32)
  * choose /26 == 64 Ips - 5 == 59 > 29
