# MSK - Managed Streaming for Kafka

* alternative to Kinesis

* fully managed Kafka on AWS
  * allows to: create, update, delete clusters
  * MSK creates && manages Kafka brokers nodes && Zookeeper nodes for you
  * deploy MSK cluster in your VPC, multi-AZ (up to 3 for HA)
  * automatic recovery from common Kafka failures
  * data is stored in *EBS* volumes *for as long as you want*
* MSK serverless
  * run without provisioning

## Kakfa Overview

* Kafa cluster made of brokers

* producers (your app) gets data from data sources
  * kinesis
  * iot
  * rds
  * etc
* sends data *topics* to brokers
  * fully replicated onto other brokers
* consumers (your code)
  * polls data from topics
  * data can be consumed or forwarded to other destinations
    * EMR
    * S3
    * SageMaker
    * Kinesis
    * RDS
    * etc

## Kinesis Data Streams vs MSK

* KDS
  * 1MB message size limit
  * data streams with shards
  * shard splitting and merging (for scale out scale in)
  * TLS in flight encryption
  * KMS at rest encryption
* MSK
  * 1MB default can configure for higher (10MB ex)
  * data streams with topics
  * can only add partitions (cannot scale in)
  * PLAINTEXT or TLS in flight encryption
  * KMS at rest encryption

## MSK Consumers

* Kinesis Data Analytics for Apache Flink: flink app reads from MSK cluster
* Glue Streaming ETL jobs powered by Apache Spark Streaming
* Lambda triggered by MSK itself
* own consumer running on: EC2, ECS, EKS
