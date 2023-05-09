# Amazon Kinesis

* makes it easy to collect, process and analyze streaming data in real-time
* ingest real-time data: app logs, metrics, wsite clickstreams
* comprised of 4 services:
  * Kinesis Data Streams: capture, process and store data streams
  * Kinesis Data Firehose: load data streams into AWS data stores
  * Kinesis Data Analytics: analyze data streams with SQL or Apache Flink
  * Kinesis Video Streams: capture, process and store video streams ⚠(️️ don't remember it being in the exam but adding it anyway)

## Kinesis Data Streams

* for streaming BigData into your systems
* data retention is between 1 day to 365 days => ability to reprocess (replay) data
* once data is inserted in Kinesis it can't be deleted (immutability)
* producers: AWS SDK, Kinesis Producer Library (KPL), Kinesis Agent
* consumer:
  * can write your own: Kinesis Client Library (KCL), AWS SDK
  * managed: Lambda, Kinesis Data Firehose, Kinesis Data Analytics
* capacity modes:
  * provisioned mode:
    * you choose the number of shard provisioned / can scale manually or usin API
    * each shard gets 1MB/s in (1000 records per second)
    * each shard gets 2MB/s out (classic or enhanced fan-out consumer)
    * pay per shard provisioned per hour
  * on-demand mode:
    * no need to provision or manage the capacity
    * default capacity provisioned (4MB/s in)
    * scales automatically based on observed throughput peak during *the last 30 days*
    * pay per stream per hour && data in/out per GB

## Kinesis Data Streams Security

* control access / authorization using IAM policies
* encryption in flight using HTTPS endpoints
* encryption at rest using KMS
* can implement encryption/decryption of data on client side
* VPC Endpoints available for Kinesis
* monitor API calls using CloudTrail
