# AWS Shield - protect from DDoS

* DDoS - Distributed Denial of Service => overload infra
* AWS Shield Standard
  * free activated for every customer
  * protects from attacks such as SYN/UDP flood, reflection attacks and other Layer 3/4 attacks
* AWS Shield Advanced
  * optional DDoS mitigation service ($3K/mo per org)
  * protects against more sophisticated attacks on EC2, LB, CloudFront, Global Accelerator and Route53
  * 24/7 access to AWS DDoS response team
  * protects against higher fees during usage spikes due to DDoS
  * automatic app layer DDoS mitigation automatically creates/evaluates/deploys AWS WAF rules for layer 7
