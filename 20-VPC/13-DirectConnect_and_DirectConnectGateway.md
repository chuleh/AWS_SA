# DirectConnect (DX)

* provide a dedicated *private* connection from remote network to your VPC
* dedicated connections must be setup between your DC and AWS Direct Connect locations
* need to set up ad VPG on your VPC
* access public resources (s3) and private (EC2) on the same connection
* use cases:
  * increase morebandwith throughput - working with large dataset - lower cost
  * more consistent network experience - applications using real-time data feeds
  * hybrid environments (on-prem + cloud)
* supports ipv4 and 6

## DirectConnect Gateway

* you want to set up DirectConnect to one or more VPC in many diff regions (same account)

## Connection Types

* dedicated connections: 1/10/100 Gbps capacity
  * get physical ethernet port dedicated to a customer
  * request made to AWS first and completed by AWS DirectConnect partners
* hosted connections: 50Mbps / 500Mbps / 10Gbps
  * connections requests are made via AWS DirectConnect partners
  * capacity can be added or removed on demand
  * 1, 2, 5, 10 Gbps available at select AWS DirectConnect partners
* lead times are often longer than 1 month to establish a new connection

## Encryption

* data in transit *is not* encrypted but it is private
* DirectConnect + VPN provides and IPsec-encrypted private connection
* good for security but slightly more complex

## Resiliency

* High resiliency for critical workloads
  * one connection at multiple locations
* Maximum resiliency for critical workloads
  * 2 or more connections for multiple locations
  * achieved by separate connections terminating on separate devices in more than one location

## DirectConnect and Site2Site VPN as backup

* in case DX fails:
  * can set up a DX connection as backup (expensive)
  * site2site vpn connection -always available since it goes thru public internet-
