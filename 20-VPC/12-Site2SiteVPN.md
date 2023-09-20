# Site to Site VPN

* connecto local datacenter with aws

* Virtual Private Gateway (VGW)
  * VPN concentrator on the AWS side of the connection
  * VGW is created and attached to the VPC from which you want to create the s2s vpn connection
  * can customize the ASN (Autonomous System Number)
* Customer Gateway (CGW)
  * software app or physical device on the customer side of the vpn

## CGW (on-prem)

* what ip to use?
  * public internet routable IP from your CGW device
  * behind a NAT => must be enabled for NAT Traversal (NAT-TA) => use public ip of NAT
* *must* enable route propagation on your VPC
  * *RoutePropagation* for the VGW in the route table that is associated with your subnets
* if you need to ping EC2 from on-prem => enable ICMP on SG for inbound

## AWS VPN CloudHub

* provide secure communication between multiple sites if you have multiple VPN connections
* low-cost hub-and-spoke model for primary or secondary network connectivity between diff locations (vpn only)
* goes thru internet since it's a VPN connection
