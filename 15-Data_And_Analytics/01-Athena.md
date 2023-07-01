# Amazon Athena

* serverless query service to analyze data stored in S3
* uses standard SQL language to query the files (built on Presto)
* supports CSV, JSON, ORC, Avro and Parquet
* pricing: $5 for TB of data scanned
* commonly used with Amazon Quicksight for reporting/dashboards
* uses cases:
  * business intelligente / analytics / reporting
  * analyze && query vpc flow logs / elb logs / cloudtrail trails
  * *any time* you need to analyze data in S3 *using serverless* SQL == Athena

## Performance improvement

* use columnar data for cost-savings (less scan)
  * recommended formats:
    * Apache Parquet
    * ORC
  * huge performance improvement
  * use glue to convert your data to Parquet or ORC
* compress data for smaller retrievals (bzip2, gzip, lz4, snappy, zlip, zstd)
* partition datasets in S3 for easy querying on virtual columns
* use larger files (> 128 MB) to minimize overhead

## Federated Query

* allows you to run SQL queries across data stored in relational, non-relational object and custom data sources (onprem or AWS)
* uses data source connectors that run on AWS Lambda to run Federated Queries (CloudWatch Logs, DynamoDB, RDS)
