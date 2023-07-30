# AWS Config

* helps with auditing and recording *compliance* of your AWS resources
* helps record configurations and changes over time
* questions solved by AWS Config
  * unrestricted ssh access to my SGs?
  * do buckets have public access?
  * how has my ALB config changed over time?
* can receive alerts (SNS) for any notifications
* per-region service
* can be aggregated across regions and accounts
* can store the config data into S3 (analyzed by Athena)

## Config Rules

* AWS managed config rules (over 75)
* can make custom rules (must be defined in Lambda)
  * evaluate if each EBS disk is of type gp2
  * evaluate if each EC2 instance is t2.micro
* rules can be evaluated/triggered
 * for each config change
 * and/or at regular time intervals
* *DOES NOT* prevent actions from happening (no deny)
* pricing:
  * no free tier
  * 0,003 per config item recorded per region
  * 0,001 per config rule evaluation per region

# Remediations

* automate remediation of non-compliant resources using *SSM Automation Documents*
  * ex: SSH keys over 90s days
  * monitored by AWS Config => trigger => AutoRemediation Action
  * SSM Document => RevokeUnusedCredentials
  * deactive SSH keys
* use AWS managed Automation Documents or create Custom ones

## Notifications

* use EventBridge to trigger notifications when resources are non-compliant
* ability to send configuration changes and compliance state notifications to SNS
  (all events - use SNS filtering or filter at client side)
