# AWS Network Firewall

* protect your entire AWS VPC
* from layer 3 to layer 7
* can inspect any kind of traffic in any direction
  * vpc to vpc
  * outbound to internet
  * inbound from internet
  * to/from DirectConnect && Site-to-site VPN
* internally uses AWS GW Load Balancer
* rules can be centrally managed cross-account by Firewall Manager to apply to many VPCs

## Fine grained controls

* supports 1000s of rules
  * IP & Port - 10K ip filtering
  * protocol - disable SMB for outbound comms
  * stateful domain list rules groups:
    * only allow outbound traffic to *.mydomain.com or third party software repo
  * general pattern matching with regex
  * traffic filtering for matches
    * allow
    * drop
    * alert
  * active flow inspection
    * intrusion-prevention capabilities (like GW LB but managed by AWS)
  * send logs of rule matches to S3 / CW Logs / Kinesis Data Firehose

