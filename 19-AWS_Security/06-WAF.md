# Web Application Firewall

* protects at layer 7 (webapps)
* deploy on
  * LB
  * API GW
  * CloudFront
  * AppSync GraphQL API
  * Cognito User Pool
* define web ACLs rules:
  * IP Set: up to 10K IP addresses - use multiple rules for more IPs
  * HTTP headers, HTTP body or URI strings protects from common attacks
    * SQL Injection
    * XSS
  * size-constraints
  * geo-match
  * rate-based rules (count occurrences of events) - good for DDoS

* web ACL are *regional except for CloudFront*
* a rule group is a *reusable set of rules* you can add to a web ACL

## Fixed IP while using WAF with a Load Balancer

* WAF does not support Network Load Balancer (layer 4)
* use *Global Accelerator* for fixed IP and WAF on the ALB
