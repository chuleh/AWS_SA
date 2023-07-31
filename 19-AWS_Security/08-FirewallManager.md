# AWS Firewall Manager

* manage rules in *all accounts* of an Org
* security policy: common set of security rules
  * WAF rules (ALB, API GW, CloudFront)
  * AWS Shield Advanced (ALB, CLB, NLB, EIP, CloudFront)
  * Security groups
  * AWS Network Firewall (VPC level)
  * Route53 Resolver DNS Firewall
  * Policies created at *region* level

* rules are applied to new resources as they are created
  * good for compliance
  * across all and future accounts in your Org
