# Amazon SNS

* when you want to send one message to many receivers
* Pub/Sub
  * eg: buying service => publish to SNS Topic => Subscribers (different services) receive message
* *event producer* only sends message to one SNS topic
* as many *event receivers* (subscriptions) as we want to listen to the SNS topic notifications
* each sub to the topic will get all the messages (note: new feature to filter messages)
* up to 12.5M subs per topic
* 100K topics limit
* SNS subs
  * emails
  * sms && mobile
  * http endpoints
  * SQS
  * Lambda
  * Kinesis / Firehose
* SNS Receives from
  * cloudwatch
  * Lambda
  * ASG
  * RDS Events
  * CloudFormation
  * etc

## How to Publish

* Topic publish
  * create topics
  * create a sub (or many)
  * publish to the topic
* Direct Publish
  * create a platform app
  * create a platform endpoint
  * publish to the platform endpoint
  * works with Google GCM, Apple APNS, Amazon ADM, etc

## SNS Security

* security:
  * in-flight encryption using HTTPS API
  * at-rest encryption using KMS keys
  * client-side encryption if the client wants to perform encryption/decryption itself
* access controls:
  * IAM policies to regulate access to SNS API
  * SNS access policies
    * similar to S3
    * useful for cross-account access to SNS topics
    * useful for allowing other services (S3, etc) to write to an SNS topic
