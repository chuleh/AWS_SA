# Networking Costs in AWS

* any incoming traffic into EC2 is *free*
  * any traffic between instances *inside the same AZ using private IPs*
* same region *but* traffic between 2 diff AZs
  * $0,02 if using Public/EIP (leaves AWS network and goes back in)
  * $0,01 if using private ip
* traffic between regions == $0,02

⚠️ use PrivateIP instead of Public/EIP for good savings and better network performance
⚠️ use same AZ for maximum cost saving *but* no HA

## Egress traffic -minimizing cost-

* keep as much traffic inside AWS to minimize costs
* ex: DB on AWS <==> queried from app in Corporate DC approx 100MB in query == high egress cost
  * move app into EC2 and same AZ as RDS and query from DC == lower egress cost since it only retrieves data from the query
* DirecConnect location that are co-located in the same AWS Region result in lower cost for egress network

## S3 data transfer pricing

* S3 ingress == free
* S3 to internet == $0,09 per GB
* S3 transfer acceleration
  * faster transfer times (50 to 500x better)
  * additional cost on top of data transfer pricing == +0,04 to 0,08 per GB
* CloudFront to internet == $0,085 per GB (slightly cheaper than S3)
  * caching capability (lower latency)
  * reduce costs associated with S3 requests pricing (7x cheaper with CloudFront)
  * S3 cross region replication == $0,02 per GB

## Nat GW vs Gateway VPC Endpoint

* scenario:
  * Private EC2 instance access S3 bucket through ==> NatGW ==> IGW ==> internet ==> S3 bucket
    * cost:
      * $0,045 nat gw / hour
      * $0,045 nat gw data processed / GB
      * $0,09 data transfer out to S3 (cross-region)
      * $0 data transfer out to S3 (same region)
  * set vpc-endpoint
    * EC2 instance ==> VPC Endpoint ==> S3 bucket
      * no cost for using GW Endpoint
      * $0,01 data transfer in/out (Same region)
