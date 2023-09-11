# NAT Instances -outadated but might still appear at the exam-

* use NATGW over this but still might show up

* NAT == Network Address Translation
* allows EC2 instances in private subnets to connect to the internet
* *must* be launched in a public subnet
* *must* disable EC2 setting `Source/Destionation check`
* must have *EIP* attached to it
* RouteTables must be configured to traffic from Private Subnets to the NAT instance
* pre-configured amazon linux AMI available -reached EOL on Dec 2020-
* not HA and resilient out of the box
* must manage SG && rules on your own
  * inbound
    * allow http/https from private subnets
    * allow ssh from home network (provided thru IGW)
  * outbound
    * allow HTTP/HTTPS to the internet
