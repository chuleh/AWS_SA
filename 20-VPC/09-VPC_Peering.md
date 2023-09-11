# VPC Peering

* privately connect 2 VPCs using AWS network
* make them behave as if they were in the same network
* must not have overlapping CIDRs
* *NOT Transitive* == must be established for each VPC that wants to connect to each other
* *must* update route tables in *each* VPC subnets to ensure EC2 instances communicate with one another

## Good to know

* is cross account
* is cross region
* you can reference an SG in a peered VPC
