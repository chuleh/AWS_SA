# Blocking an IP on AWS

* common cases
  * client => NACL at VPC level => SG around EC2 instance with EIP + *optional* Firewall Software in EC2
    * block client == NACL => DENY rule for IP
    * SG for instance => no DENY rules => SG allow rules for a subset of IPs (if known)
    * EC2 FW SW => add to block from client => add cost since the request already reached the instance and needs to be processed
  * client => NACL at VPC level => ALB security group => EC2 SG => EC2 with private IP
    * first line of defense at NACL
    * ALB with *ConnectionTermination* => terminates connection from outside and initiates a new one to EC2
    * EC2 SG must allow ALB
    * can *add WAF* => IP address filtering
      * service attached to ALB
    * same pattern for *NLB*
  * CloudFront with WAF
    * CloudFront Geo restriction => restric country
    * can have WAF IP address filtering => restrict IP
    * NACL *is not* helpful
    * ALB allow CloudFront Public IPs => ALB => same pattern as #8

