# SNS + SQS: Fan out

* Push once in SNS => receive in all SQS queues that are subscribers
* fully decoupled
* no data loss
* SQS allows for: data persistence, delayed processing, retries of work
* ability to add more SQS subscribers over time
* cross-region delivery: works with SQS queues in other regions
⚠️  must make sure SQS queue *access policy* allows for *SNS to write*

## S3 events into multiple queues

* for the same combination of: *event type* (eg: object create) and *prefix* (eg: images/) you can only have *one S3 event rule*
* if you want to send the same S3 event to many SQS queues => *use fan out*

⚠️  Also: SNS can send to kinesis and from there to S3

## SNS + FIFO

* topic name must end in *.fifo*
* ordering by MessageGroup ID (all messages in the same group are ordered)
* deduplication using a deduplication ID or Content Based Deduplication
* *can only have SQS FIFO queues as subscribers*
* limited throughput (same as FIFO queue)
* useful when you need: fan out + ordering + deduplication

## SNS Message Filtering

* JSON policy used to filter messages sent to SNS topics subscribers
* if a sub doesn't have a filter policy it receives every message
