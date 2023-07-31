# Amazon GuardDuty

* intelligent threat discovery to protect your AWS account
* uses Machine Learning algorithms, anomaly detection, 3rd party data
* one-click enable (30 days trial) - no software needed
* input data includes:
  * CloudTrail Events Logs - unusual api calls, unauthorized deployments
    * CloudTrail Management Events - create vpc subnet, create trail, etc
    * CloudTrail S3 Data Events - get object, list objects, delete object
  * VPC flow logs - unusual internal traffic, unusual IP address
  * DNS logs - compromised EC2 instances sending encoded data with DNS queries
  * optional features - EKS audit logs, RDS && Aurora, EBS, Lambda, S3 Data Events
  * EventBridge rules can target Lambda or SNS
  * can protect against CryptoCurrency attacks - has dedicated finding for it
