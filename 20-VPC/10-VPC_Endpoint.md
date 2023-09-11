# VPC Endpoint -AWS PrivateLink-

* *every* AWS service is publicly exposed (public URL)
* deployed within VPC
* access resources privately -from within VPC- (not through internet traffic)
* redundant and scale horizontally
* removes need of IGW or NATGW to access other resources
* needs role or policy to be attached -policy can be empty-
* if vpc-endpoint set up and using CLI for S3 remember to change region
  * `aws s3 ls --region=myregion`
* in case of issues:
  * check DNS setting resolution in VPC
  * check Route Tables

## Types of endpoints

* Interface Endpoints (powered by PrivateLink)
  * provisions an ENI (private IP address) as an entry point (must attach an SG)
  * supports most AWS services
  * cost $ per hour + $ per GBof data processed
* Gateway endpoint
  * provisions a gateway and must be used *as a target in a route table || does not use SG*
  * supports both S3 and DynamoDB
  * free
* good to know:
  * gateway or interface for S3?
    * interface endpoint is preferred if access required from:
      * site-to-site VPN
      * DirectConnect
      * diff VPC or diff region
