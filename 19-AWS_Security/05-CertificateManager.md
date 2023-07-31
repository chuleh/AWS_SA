# AWS Certificate Manager (ACM)

* provision, manage, deploy TLS Certificates
* provide in-flight encryption for websites (HTTPS)
* supports both public and private TLS Certificates
* free of charge for public TLS Certificates
* automatic TLS Certificates renewal
* integrations
  * ELB (CLB, ALB, NLB)
  * CloudFront distributions
  * APIs on API Gw
* cannot be used with EC2

## requesting public certification

* list domain names to be included in the certificate
  * FQDN
  * Wildcard Domain: *.example.com
* select validation method: DNS validation or Email validation
  * DNS validation is preferred for automation purposes
  * Email validation will send emails to contact addresses in the WHOIS database
  * DNS validation will leverage a CNAME record to DNS config (ex: Route53)
* will take a few hours to get verified
* Public Certificate will be enrolled for automatic renewal
  * ACM automatically renews ACM-genereated certificates 60 days before expiry

## importing public certification

* option to generate the certificate outside of ACM and then import it
* *no auto renewal* => must import a new certificate before expiry
* ACM sends daily expiration events starting 45 days prior to expiration
  * number of days can be configured
  * event appear in EventBridge
* ACM Config has a managed rule named *acm-certificate-expiration-check* to check for expiring certs

## integration with ALB

* provisions and maintains TLS certs
* if going thru HTTP
  * ALB redirects to HTTPS
  * user goes thru HTTPS

## integration with API GW

* create *Custom Domain Name* in API GW
* Edge-Optimized endpoints (default)
  * for global clients
  * requests are routed thru CloudFront Edge locations (improves latency)
  * API GW still lives in only one region
  * TLS certificates must in the same region as CloudFront - us-east-1
  * set up a CNAME or (better) A-Alias record in Route53
* regional
  * for clients in the same region
  * TLS Certificates must be imported on API GW in the same region as the API Stage
  * set up a CNAME or (better) A-Alias record in Route53
