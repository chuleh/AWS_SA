# CloudTrail

* provides governance, compliance and audit for you AWS Acct
* *enable by default*
* get history of events / API call made within your AWS Acct by:
  * console
  * SDK
  * CloudTrail
  * AWS Services
* can put logs from CloudTrail into CW Logs or S3
* A trail can be applied to All Regions (default) or a single region
* if resource deleted in AWS => check CloudTrail first

## CloudTrail Events

* Management Events
  * operations that are performed on resources in your AWS Account
  * ex:
    * configuring security (IAM *AttachRolePolicy*)
    * configuring rules for routing data (Amazon EC2 *CreateSubnet*)
    * setting up logging (AWS CloudTrail *CreateTrail*)
  * by default *trails are configured to log management events*
  * can separate *Read Events* from *Write Events*

* Data Events
  * by default data events are *not logged* (due to high voulume operations)
  * Amazon S3 object-level activity (ex: GetObject, DeleteObject, PutObject): can separe RO from RW
  * AWS Lambda function execution activity (*Invoke* API)

* CloudTrail Insights

* *has to be enabled* (extra cost)
* useful for detecting unusual activity in your account
  * innacurate resource provisioning
  * hitting service limits
  * bursts of AWS IAM actions
  * gaps in periodic maintenance activity
* CloudTrail Insights analyzes normal management events to create a baseline
  * then continuosly analyzes *write* events to detect unusual patterns
    * anomalies appear in CloudTrail console
    * Event is sent to Amazon S3
    * an EventBridge event is generated (for automation needs)

## CloudTrail Events retention

* stored for 90 days in CloudTrail
* to keep beyond period => log to S3 and use Athena
