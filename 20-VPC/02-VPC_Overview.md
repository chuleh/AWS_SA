# VPC Overview

* VPC == Virtual Private Cloud
* all new AWS accounts have a default VPC
* new EC2 instances are launched into the default VPC if no subnet is specified
* default VPC has internet connectivity - all instances inside have a public IPv4
* also get public and private IPv4 DNS names
* can have multiple VPCs in AWS region (max 5 per region - soft limit)
* max CIDR per VPC is 5, for each CIDR:
  * min size /28 (16 ips)
  * max size /16 (65536)
* because VPC is private only IPv4 ranges are allowed
  * 10.0.0.0/8
  * 172.16.0.0/12
  * 192.168.0.0/16
* general rule: VPC CIDR *must not* overlap  with other networks
