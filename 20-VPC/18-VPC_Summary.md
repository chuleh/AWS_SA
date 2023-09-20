# VPC Summary

* CIDR - IP Range
* VPC - Virtual Private Cloud => we define a list of IPv4 && IPv6 CIDR
* Subnets - tied to a VPC, we define a CIDR
* Internet Gateway (IGW) - at VPC level, provides IPv4 && IPv6 internet access
* Route Tables - must be edited to add routes from subnets to IGW, VPC Peering, VPC Endpoints, etc
* Bastion Host - public EC2 instance to SSH into and has connectivity to SSH into private EC2
* NAT instances - gives internet access to private EC2 instances in private subnets *Deprecated*
  * must be set up in a public subnet and disable Source / Destination check flag
* NAT GW - managed by AWS - provides scalable internet access to private EC2 instances - IPv4 only
* NACL - stateless, subnet rules for inbound/outbound - *don' forget ephemeral ports*
* Security Groups - stateful, operate at instance level
* VPC peering - connect two VPCs with non-overlapping CIDR - non-transitive
* VPC endpoints - provide private access to AWS Services (S3, DynamoDB, CloudFormation, SSM) within a VPC
* VPC Flowlogs - can be set up at VPC / Subnet / ENI level for ACCEPT and REJECT traffic
  * helps identify packets of attacks
  * analyze using Athena or CloudWatch Logs Insights
* Site-to-Site VPN - set up a Customer Gateway (CGW) on a DC, a Virtual Private Gateway (VGW) on VPC and s2s vpn over public internet
* AWS VPN CloudHub - hub and spoke VPN model to connect your sites
* DirectConnect - set up a Virtual Private Gateway (VGW) on a VPC and establish a direct private connection to an AWS Direct Connect location
  * takes a lot of time but it' private - doesn' go over internet
* DirectConnect Gateway - set up a DirectConnect to many VPCs in many different regions
* AWS PrivateLink / VPC Endpoint Services - connect services privately from your service VPC to your customers VPC
  * doesn't need VPC peering, public internet, NAT GW, Route Tables
  * *must* be used with Network Load Balancer (NLB) and ENI
* ClassicLink - connect EC2-classic instances privately to your VPC
* Transit Gateway - transitive peering connections for VPC, VPN, DX (DirectConnect)
* TrafficMirroring - copy network traffic for further analysis
* Egress-only Internet Gateway - NAT Gateway for IPv6
