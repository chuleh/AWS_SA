# SQS vs SNS vs Kinesis

* SQS
  * consumer pull data
  * data deleted after consumed (so no other consumer reads it)
  * as many workers (consumers) as needed
  * no need to provision throughput
  * ordering only guaranteed on FIFO queues
  * individual messsage delay capability
* SNS
  * push data to many subs
  * up to 12.5M subs
  * data not persistent == lost if not dlivered
  * up to 100K topics
  * no need to provision thruput
  * integrates with SQS for fan-out arch
  * FIFO capability for SQS FIFO
* Kinesis
  * standard: pull data
    * 2MB per shard
  * enhanced fan-out: push data
    * 2MB per shard per consumer
  * possibility to replay data
  * meant for real time BigData analytics and ETL
  * ordering at the shard level
    * specify how many shards in advance
    * need to scale shard yourself
    * data expires after X days
    * provisioned mode or on-demand capacity mode
