# Event Processing

## SQS && Lambda

* SQS + Lambda
  * events into SQS
  * Lambda polls SQS
    * in case of error puts back into SQS => can go into infinite loop of try/retry
  * create DLQ (DeadLetterQueue) for SQS after X tries
* SQS FIFO + Lambda
 * events into SQS
 * Lambda receives them *but* what if it message 1 gets blocked?
   * blocks all queue since it's FIFO
 * create DLQ (DeadLetterQueue) for SQS after X tries

## SNS && Lambda

* SNS + Lambda
  * SNS async => Lambda
  * Lambda receives SNS and retries 3 times (internally)
    * if not processed == discarded
    * set up DLQ at Lambda service level
    * sent to SQS

## Fan Out Pattern: deliver to multiple SQS

* option 1
  * App with SDK
    * send message to SQS1 - SQS2 - SQS3
    * unreliable == app crashes sending to any SQS => messaging stops
* option 2
  * App with SDK
    * send message to SNS
    * SNS subbed to diff SQS
    * fans out messages to different SQS

## S3 Event notification

* react on notifications
  * ObjectCreated / ObjectRemoved / ObjectRestored / etc
  * object name filtering possible (*.jpg)
  * use case: generate thumbnails of images uploaded to S3S (with a Lambda)
  * can create as many S3 events as desired
  * event notifications typically deliver events in seconds but can take up to a minute or longer

## S3 Event notification with Amazon EventBridge

* event on S3 => delivered to EventBridge
  * add rules => send to over 18 services as destinations
* add filtering options with JSON rules (metadata, object size, name, etc)
* multiple destinations at once - step functions, kinesis streams / firehose, etc
* capabilites
  * archive
  * replay events
  * reliable delivery

## EventBridge - intercept API calls

* user - DeleteTable API Call onto DynamoDB
* CloudTrail logs API call
* trigger event on EventBridge
* create SNS alert

## API Gateway - service integration

* client requests to API Gateway
* API Gateway => send to Kinesis Data Streams
* records delivered to Kinesis Data Firehose
* finally delivered to S3

