# Best practices for DDoS mitigation

* best practices at edge
  * User => CloudFront (web app delivery at the edge)
    * protection from DDoS by default with Shield standard
  * User => Global Accelerator (access app from the edge)
    * protection from DDoS by default with Shield standard
    * helpful if backend is somehow not compatible with CloudFront
  * User => Route53
    * domain name resolution at the edge
    * protection from DDoS by default with Shield standard
* best practices infrastructure layer
  * Start from either CloudFront/Global Accelerator/Route53
    * Elastic Load Balancing
      * protect EC2 against high traffic => auto scales and distributes traffic
    * AutoScaling
      * scale EC2 in case of traffic spike
* best practices application layer
  * detect and filter malicious requests
    * CloudFront cache static content and serve it from edge locations => protects backend
      * can block specific geographies
    * WAF on top of CloudFront and ALB to filter and block requests based on signatures
      * rate-rules can automatically block bad IPs
      * use managed rules to block IPs based on reputation or anonymous IPs
    * shield advanced
* best practices to reduce attack surface
  * obfuscate aws resources
    * CloudFront/API GW/ELB => hide backend resources (EC2, Lambda)
  * Security groups and network ACLS
    * filter traffic based on specific IP at the subnet or ENI level
    * EIP is protected by aws shield advanced
  * protecting API endpoints
    * hide EC2, Lambda endpoints
    * edge-optimized mode or CloudFront + regional mode
    * WAF + API GW: burst limits, headers filtering, use api keys
