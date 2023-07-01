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

## OpenSearch Patterns

### DynamoDB

* DynamoDB (stores data) => send all data thru DynamoDB Stream
  * data sent to a Lambda Function => inserts all data into OpenSearch
  * app in EC2 => searches items by item name in OpenSearch
  * data is obtained and calls DynamoDB to obtain *full name* of the item

### CloudWatch Logs

* CloudWatch logs with *subscription filter*
  * pushes data onto Lambda (in real time)
  * lambda in real time sends data to OpenSearch
* CloudWatch logs with *subscription filter*
  * sends data to Kinesis Data Firehose
  * in near real time pushes data to OpenSearch

### Kinesis Data Streams and Kinesis Data Firehose

* Kinesis Data Streams => pushes to Kinesis Data Firehose
  * Firehose (in near real time) optionally pushes to lamba for data trasnformation
  * data sent back to Fireshose
  * Firehose pushes to OpenSearch
* Kinesis Data Streams pushes to Lambda (real time)
  * lambda *needs to* have custom code to write to => OpenSearch
