# ECS - Solutions Architecture Discussion

* ECS tasks invoked by Event Bridge
  * client uploads object on S3 bucket
  * integrated with Event Bridge to send all events
  * Event Bridge has a rule to *Run ECS Task*
  * task has ECS Task Role to access S3 and DynamoDB
    * gets object from S3 and process it
    * save result in DynamoDB
* ECS tasks invoked by Event Bridge Schedule
  * Schedule a rule to run every 1 hour
    * rule: Run ECS task
    * ECS task role to access S3
    * ex: do batch processing and upload to S3
* ECS - SQS Queue Example
  * messages sent to SQS queue
  * enable ECS service autoscaling
  * > messages => > taks on ECS service autoscaling
* ECS - intercept stopped tasks using Event Bridge
  * react to a task being exited
  * send event to Event Bridge (StoppedReason)
  * Event Bridge *triggers* SNS
  * SNS emails admin
