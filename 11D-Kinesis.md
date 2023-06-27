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
    * no need to provisio or manage the capacity
    * default capacity provisioned (4MB/s in)
    * scales automatically based on observed throughput peak during *the last 30 days*
    * pay per stream per hour && data in/out per GB

## Kinesis Data Streams Security

* control access / authorization using IAM policies
* encryption in fligh using HTTPS endpoints
* encryption at rest using KMS
* can implement encryption/decryption of data on client side
* VPC Endpoints available for Kinesis
* monitor API calls using CloudTrail

## Kinesis Data Firehose

* fully managed
* no administration
* automatic scaling
* serverless
* *Near Real Time* service
  * 60 seconds latency minimum for non full batches
  * 1 MB of data at a time
* Pay for data going thru firehose
* support many data formats, conversions, transformations, compression
* supports custom data transformations using aws lambda

* loads data streams into data stores
  * producers can be:
    * apps / kinesis agent / kinesis streams / CloudWatch Event / AWS IoT / Client /SDK KPL
  * all of these apps send a *record* (1MB max) to *Data Firehose*
  * data *can* trasnformed by *Firehose* into a Lambda
  * finally written in *batches* into destinations
  * destinations:
    * AWS:
      * S3
      * Amazon RedShift (copy thru S3)
      * Amazon OpenSearch
    * 3rd party:
      * DataDog
      * Splunk
      * NewRelic
      * MongoDB
    * Custom Destinations
      * http endpoint

* All or failed data can be sent to a S3 backup bucket

### Kinesis Data Firehose notes

* S3 buffer hints:
  * Buffer size: 1MB to 128MB => lower buffer higher delivery && +cost && -latency|| bigger buffer lower delivery && -cost && +latency
  * buffer interval: > interval == more time to collect data && size of data may be bigger || < interval == sends data more freq && *may be* advantegeous for shorter cycles of data activity
    * 60 secs min - 900 secs max
  * Permissions: must have a role. Either created on the fly or existing one to read/write to S3.

### Kinesis Data Streams vs FireHose

* KDS
  * Streaming service for ingest at scale
  * write custom code (producer / consumer)
  * real time (~200 ms)
  * manage scaling (sharding / splitting)
  * data storage for 1 to 365 days
  * supports replay capability
* KDF
  * Load streaming data into S3 / RedShift / OpenSearch / 3rd party / custom http
  * fully managed
  * *near real time*
  * AS
  * *NO* data storage == no replay
