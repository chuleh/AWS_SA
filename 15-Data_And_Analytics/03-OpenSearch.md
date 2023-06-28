# OpenSearch

* succesor to ElasticSearch
* in DynamoDB queries only exist by primary key or indexes || with opensearch you can search *any* field even *partial matches*
* common to use opensearch as a complement to another database
* two modes:
  * managed cluster
  * serverless cluster
* does not natively support SQL (can be enabled via plugin)
* can ingest data from Kinesis Data Firehose, IoT, and CloudWatch Logs
* security through Cognito && IAM, KMS encryption, TLS
* comes with OpenSearch Dashboards (visualization)