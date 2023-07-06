# CloudWatch Logs

* stores app logs
* log groups: arbitrary name => usually represents an app
* log stream: instances within an app / log files / containers
* can define log expiration policy (never expire, 1 day to 10 days, etc.)
* can send logs to:
  * S3 (exports)
  * KDS
  * KDF
  * Lambda
  * OpenSearch
* encrypted by default
* can set up KMS-based encryption with own keys

## Sources

* SDK / CloudWatch Logs Agent / CloudWatch Unified Agent
* ElasticBeanstalk: collection of logs from application
* ECS: collection from containers
* Lambda: collection from function logs
* VPC Flow Logs: vpc specific logs
* API Gateway: requests made to api gw
* CloudTrail: logs based on filter
* Route53: log DNS queries

## CloudWatch Logs Insights

* search and analyze log data stored in CW Logs
* ex: find a specific IP inside a log / count occurrences of "ERROR" in a log
* provides a purpose-built query language
  * automatically discovers fields from AWS services and JSON log events
  * fetch desired event fields, filter based on conditions, calculate aggregate statistics, sort events, limit number of events
  * can save queries and add them to CloudWatch Dashboards
* can query multiple log groups in different AWS accounts
* it's a query engine *not* a real-time engine

## S3 Export

* batch export sends logs to S3
* can take up to 12hours to become available for export
* API call is *CreateExportTask*
* *not* near real time or real time (use Log Subscriptions instead)

## Log Subscriptions

* get real time log events from CW Logs for processing and analysis
* send to KDS, KDF or Lambda
* subscription filter - filter which logs are events sent to destination
  * sent to lambda (custom code) => sent to OpenSearch
  * KDF near real time sent to => S3 || OpenSearch
  * KDS sent to => KDF / KDA / EC2 / Lambda

### Aggregation - Multi-account && Multi-Region

* Cross-Account Subscription - send log events to resources in different AWS accounts
* sender account: sends from CW Logs to Sub Filter
  * destination account has *subscription destination* forwards to KDS (recipient stream)
  * destination sub *has* destination access policy allowing sender account
  * must have IAM role that allows *PutRecord* that sender account can assume the role
