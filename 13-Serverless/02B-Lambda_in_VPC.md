# Lambda VPC

* by default launched outside your VPC
* cannot access resources in your VPC

## Launch Lambda in VPC

* must define the VPC ID / subnets / SGs
* lambda will create an ENI (Elastic Network Interface) in your subnets
* will have private connectivity within vpc

## Lambda with RDS proxy

* Lambda functions directly accessing DB => may open too many connections under high load
  * RDS proxy
    * improves scalability by pooling and sharing db connections
    * improves availability by reducing 66% the failover time by preserving connections
    * improves security by enforcing IAM auth and storing creds in Secrets Manager
* Lambda mnust be deployed in your VPC *RDS proxy never accessible from public*

