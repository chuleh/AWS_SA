# Amazon EventBridge - Intercept API Call

* receive notification when User deletes DynamoDB table
  * *DeleteTable* api call => DynamoDB
  * DynamoDB => log api call => CloudTrail
  * CloudTrail => event => Amazon EventBridge
  * EventBridge => event => SNS
* same pattern for User => *AssumeRole* => IAM Role
* same pattern for SG inbound rule => *AuthorizeSecurityGroupIngress* => EC2 API Call Logs
