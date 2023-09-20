# VPC Flowlogs

* capture information about IP traffic going into your interfaces
  * VPC flow logs
  * subnet flow logs
  * ENI flow logs
* helps to monitor && troubleshoot connectivity issues
* flow logs data can go to S3 / CW Logs / Kinesis Data Firehose
* captures information from AWS managed interfaces too:
  * ELB
  * RDS
  * ElastiCache
  * RedShift
  * Workspaces
  * NATGW
  * TransitGW
* VPC flow logs can be quieried via:
  * athena on S3
  * CW Logs Insights

## Troubleshooting logs

* Check the `action` field
* Inbound REJECT => NACL or SG
* Inboud ACCEPT, Outboud REJECT => NACL
* Outbound REJECT => NACL or SG
* Outbound ACCEPT, Inbound REJECT => NACL

## VPC Flow Logs Architectures

* VPC FL => CW Logs => CW Contributor Insights (maybe check top 10 ips)
* VPC FL => CW Logs => custom metric -rds ssh- => trigger CW Alarm => Alert on SNS
* VPC FL => S3 => Amazon Athena (sql query) => Amazon Quicksight (visualization)
