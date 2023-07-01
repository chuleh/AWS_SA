# Kinesis Data Analytics

## for SQL Apps

* becoming *legacy*

* real-time analytics on Kinesis Data Streams & Firehose using SQL
* add reference data from Amazon S3 to enrich streaming data
* fully managed - no servers to provision
* autoscale
* pay for actual consumption rate

* reads data from Kinesis Data Streams or Kinesis Data Firehose
* then apply *SQL statements* to perform real time data analytics
* possible to reference data from *s3 bucket*
* can send data to
  * kinesis data firehose => proxies to
    * S3
    * RedShift (COPY thru S3)
    * other firehose destinations
  * kinesis data stream (does real time processing)
    * using Lambda
    * any app running on EC2

* use cases:
  * time-series analytics
  * real-time dashboards
  * real-time metrics

## for Apache Flink

* uses Flink
* you can write your own app (Java, Scala, SQL) to process and analyze streaming data
* can read from Kinesis Data Streams or Amazon MSK
* run any Apache Flink app on a managed cluster by AWS
  * provisioning of compute resources, parallel computation, autoscaling
  * application backups (implemented as checkpoints and snapshots)
  * any apache flink programming features
  * *does not* read from Firehose
