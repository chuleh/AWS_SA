# NAT Gateway

* AWS-managed NAT, higher bandwidth, HA, no administration
* pay per hour of usage of bandwidth
* NATGW is created with an EIP in a specific AZ
* can' be used by EC2 instance in the same subnet (only from other subnets)
* requires IGW (PrivSub => NATGW => IGW)
* 5 gbps of bandwidth with autoscaling up to 100 gbps
* no SGs required or managed

## NAT GAteway with HA

* NATGW is resilient within 1 AZ => must create multiple NATGWs in multiple AZs for fault-tolerance
* there is no cross-AZ failover => if an AZ goes down doesn' need a NAT
