# AWS Lambda - Virtual Functions

* no servers to manage
* limited by time - short executions (up to 15 mins)
* run on-demand
* scaling automated
* lambda config:
  * memory
  * timeout
  * execution role

## Benefits

* Easy pricing:
  * pay per request and compute time
  * free tier of 1M Lambda Requests and 400K GB of compute time
* integrate with the whole AWS suites of services
* easy monitoring through AWS CloudWatch
* easy to get more resources per function (up to 10GB of RAM)
* increasing ram increases CPU and Network

## Language Support

* nodejs
* python
* java
* C# (.NET Core)
* Golang
* C# / Powershell
* Ruby
* Custom Runtime API (community supported, eg Rust)
* Lambda Container image
  * container image must implement the Lambda Runtime API
  * ECS / Fargate is preferred for running arbitrary docker images

## AWS Lambda Integrations

* eg: Serverless Thumbnail Creation
  * image uploaded onto S3
  * triggers Lambda that creates a thumbnail
  * pushed to S3 bucket
  * inserts metadata in DynamoDB
eg: Serverless CRON job
  * CloudWatch Event Rule (EventBridge)
  * tigger every 1 hour => Lambda to perform task

## Pricing

* pay per calls
  * first 1M requests are free
  * 0,20c per 1M requests thereafter
* pay per duration
  *  400K GB seconds of compute for free == 400K seconds if function is 1GB of RAM
  * after that $1 for 600K GB-seconds

## Lambda Limits - per region

* execution
  * memory allocation: 128MB - 10GB (1MB increments)
  * max execution time: 900 seconds (15 minutes)
  * env variables: 4KB
  * disk capacity in the "function container" (in /tmp): 512MB to 10GB
  * concurrency executions: 1000 (can be increased)
* deployment
  * function deployment size (compressed .zip): 50MB
  * size of uncompressed deployment (code + deps): 250MB
  * can use /tmp/ dir to load other files at startup
  * env variables: 4KB

