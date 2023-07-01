# Solutions Architecture Discussion

* we want:
  * fully serverless managed by AWs
  * collect data in real time
  * transform data
  * query the data trasnformed using SQL
  * reports created using queries should be in S3
  * load data created into a warehouse and create dashboards

* answer
  * using IoT (example)
  * sends data to IoT Core => near real-time to => Kinesis Data Streams
  * sends to Kinesis Data Firehose (every 1 minute offloads data) => sent to S3 (ingestion bucket)
  * use a Lambda linked to KDF to cleanse data
  * S3 ingestion bucket => triggers SQS (optional) => triggers Lambda
  * Lambda triggers => Athena SQL query <=> pulls data from S3 ingestion bucket
  * Athena (serverlss SQL service) stores => S3 Reporting Bucket
    * data can be seen from QuickSight
    * or RedShift (not serverless) - can also be an endpoint for QuickSight
