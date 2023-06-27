# Data Ordering: Kinesis vs SQS FIFO

* Case study:
  * 100 trucks sending their GPS positions to AWS regularly (truck_1, truck_2 ...)
  * you want to consume data *in order* for each truck to accurately track movement
  * How should you send data to *Kinesis* == Ordering
    * send using a *Partition Key* value of *truck_id*
    * *IF* same key => will always go to *same shard* == won't go to diff shard and can be tracked
  * How should you send data into *SQS* == Ordering
    * *no* data ordering in SQS standard
    * use: SQS FIFO
      * if no *group id* messages are consumed in the order they're sent with only *one* consumer
      * *have to* scale number of consumers && messages to be grouped when they're related to each other
      * must use *Group ID* (similar to Partition Key in Kinesis)

## Kinesis vs SQS ordering

* Case: 100 trucks, 5 kinesis shard, 1 SQS FIFO

* KDS:
  * On avg ~20 trucks per shard
  * data ordered within each shard
  * max consumers in parallel == 5
  * can receive up to 5MB/s of data
* SQS FIFO:
  * Only one SQS FIFO
  * will have 100 group id
  * up to 100 consumers (due to group id)
  * up to 300 msgs per second (or 3k if batching)
