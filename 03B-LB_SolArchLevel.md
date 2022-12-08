## LBs for Solutions Architect
* CLB: questions on SGs, stickiness
* ALB: Layer 7 of OSI
  * support routing based on hostname (users.example.com & payments.example.com)
  * support routing based on path (example.com/users & example.com/payments)
  * support redirects (from HTTP to HTTPS for example)
  * support dynamic host port mapping with ECS
* NLB: Layer 4 of OSI
  * gets a static IP per AZ
  * public facing: must attach EIP - can help whitelist by clients
  * private facing: will get random private IP based on free ones at time of creation
  * has cross zone load balancing (can route traffic among AZs)
  * SSL termination
* LB SGs:
  * SG for ALB (80 or 443) and then SG for EC2 instance allowing ALB SG (source sg-XXX)
* LB SSL:
  * Uses and X.509 certificate (SSL/TLS server certificate)
  * Can manage certificates using ACM (AWS Certificate Manager)
  * Can create or upload own certificates 
  * HTTPS Listener:
    * must specify default certificate
    * can add optional list of certificates to support multiple domains
    * clients can use SNI (Server Name Indication) to specify the hostname they reach

