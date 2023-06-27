# DynamoDB Accelerator - DAX

* fully managed, highly available, seamless in-memory cache for DynamoDB
* help solve read congestion by caching
* microsecond latency for cached data
* doesn't require application logic modification (compatible with DynamoDB APIs)
* 5 minutes TTL for cache (default)

## DAX vs ElastiCache

* DAX:
  * in front of DynamoDB
  * individual objects cache
  * query & scan cache
* ElastiCache
  * store aggregation result

## DynamoDB - Stream processing

* Ordered stream of item-level modifications (create/update/delete) in a table
* use cases:
  * react to changes in real-time (welcome email to users)
  * real-time usage analytics
  * insert into derivative tables
  * implement cross-region replication
  * invoke Lambda on changes to your DynamoDB
* DynamoDB Streams:
  * 24 hours retention
  * limited # of consumers
  * process using AWS Lambda Triggers or DynamoDB Stream Kinesis adapater
* Kinesis Data Streams:
  * newer
  * 1 year retention
  * higher # of consumers
  * process using Lambda, Kinesis Data analytics, Kinesis Data Firehose, AWS Glue Streaming ETL, etc

## DynamoDB Global Tables

* table replicated across multiple regions
* 2 way replication
* make DynamoDB table accessible with low latency in multiple regions
* Active-Active replication
* apps can *READ* and *WRITE* to the table in any region
* *must* enable DynamoDB Streams as a pre-requisite

## DynamoDB TTL

* automatically delete items after an expiry timestamp
* use cases:
  * reduce stored data by keeping only current items
  * adhere to regulatory obligations
  * web session handling

## DynamoDB - Backup for DR

* Continuous backups using PITR (point in time recovery)
  * optionally enabled for the last 35 days
  * PITR to any time within the backup window
  * recovery process creates a new table
* On-demand backups
  * full backups for long-term retention until explicitly deleted
  * doesn't affect performance or latency
  * can be configured and managed in AWS backup (enables cross-region copy)
  * recovery process creates a new table

## DynamoDB - integration with S3

* export to S3 - must enable PITR
  * works for any PITR in the last 35 days
  * doesn't affect read capacity of your table
  * perform data analysis on top of DynamoDB
  * retain snapshots for auditing
  * ETL on top of S3 data before importing back to DynamoDB
  * export in DynamoDB JSON or ION format
* import from S3
  * import CSV, DynamoDB JSON or ION format
  * doesn't consume any write capacity
  * creates a new table
  * import errors are logged in CloudWatch logs
