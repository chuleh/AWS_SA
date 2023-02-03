# Integration and Messaging

* 2 patterns of communication
  * synchronous communication => application to application
    * problematic when spike of traffic => better to decouple application using:
      * SQS: queue model
      * SNS: pub/sub model
      * Kinesis: real-time streaming model
  * asynchronous communication (event based) => application to queue to application

## Amazon SQS

* Queue: contains messages
* producer: sends messages into SQS (1 or N)
  * sends to SQS using the SDK (SendMessage API)
  * message is *persisted* in SQS until a consumer deletes it
  * message retention defaults to 4 days / max 14 days
  * ex: send an order to be processed
    * order id
    * customer id
    * any attribute you want
* consumer: process and receives messages. (polls from queue and deletes once received)
  * apps you write on code
  * can run on EC2 or own on-prem servers or can be Lambdas
  * polls from queue ("asks" if it has a message) => can receive up to 10 messages at a time
  * process the message (ex: inserts the message into an RDS DB)
  * deletes message from queue (since it was processed) => uses DeleteMessage API
  * EC2 multiple instances consumers
    * more than 1 instance
    * consumers receive and process messages in parallel
    * at least once delivery
    * best effort message ordering
    * add consumers (ASG) when you need more throughput
      * ex: 1 EC2 instance in an ASG => metric: queue length => CloudWatch Metric `ApproximateNumberOfMessages` => triggers ASG

## Amazon SQS Standard Queue

* older service (over 10 years)
* fully managed service
* unlimited throughput - unlimited number of messages in queue
* default retention of messages: 4 days / max 14 days
* low latency: < 10ms on publish and receive
* limitation of 256kb per message sent
* can have duplicate messages (at least once delivery)
* can have out of order messages (best effort ordering)

## Amazon SQS Security

* security:
  * in-flight encryption using HTTPS API
  * at-rest encryption using KMS keys
  * client-side encryption if the client wants to perform encryption/decryption itself
* access controls:
  * IAM policies to regulate access to SQS API
  * SQS access policies
    * similar to S3
    * useful for cross-account access to SQS queues
    * useful for allowing other services (SNS, S3, etc) to write to an SQS queue

## SQS Message Visibility Timeout

* after a message is polled by a consumer, it becomes *invisible* to other consumers
* by default, "message visibility timeout" is 30 seconds => message has 30 secs to be processed
  * after the timeout is over, message is *visible* in SQS
  * if not processed within invisibility windows => processed twice
  * If consumer knows it'll take longer to process message => call *ChangeMessageVisibility* API to get more time
* if visibility timeout is high (hours) and consumer crashes, reprocessing will take time
* if visibility timeout is low (seconds), may get duplicates

## SQS Long Polling

* when consumer requests messages from the queue, it can optionally "wait" for messages to arrive if there are none in the queue
* decreases the number of API calls made to SQS while increasing the efficiency and latency of your app
* wait time can be between 1 sec to 20 secs (20 secs preferred)
* *long polling* is preferred to *short polling*
* can be enabled at the queue level or the API level using *WaitTimeSeconds*

## SQS FIFO Queues

* FIFO: First In First Out (ordering of messages in the queue)
* limited throughput: 300 msg/s without batching - 3000 msg/s with
* exactly-once send capability (by removing dups)
* messages are processed in order by the consumer
* must end in *.fifo* the name of the queue

## SQS + ASG

* SQS as a buffer to DB Writes
  * Receiving a big load of messages (ex: marketing campaign) EC2 instead of writing directly to DBs sends messages to SQS (infitely scalable)
  * request goes to EC2 => to SQS => to another ASG to dequeue msgs => insert into RDS && delete once message is written
* SQS to decouple between application tiers
  * requests go to frontend webapp (with ASG) =>  send msg to SQS Queue => Backend processing app (with ASG) receives message and process it

