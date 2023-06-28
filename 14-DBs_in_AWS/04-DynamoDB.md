# DynamoDB

## Summary

* AWS propietary technology, managed serverless NoSQL DB, millisecond latency
* capacity modes: provisioned capacity with optional auto-scaling or on-demand capacity
* can replace ElastiCache as a key/value store (storing session data for example, using TTL feature)
* highly available / multi az by default / reads and writes are decoupled / transaction capability
* DAX cluster for read cache, microsecond read latency
* security: authentication and authorization is done through IAM
* event processing: DynamoDB Streams to integrate with Lambda or Kinesis Data Streams
* global table feature: active-active setup
* automated backup up to 35 days with PITR (restore to new table) or on-demand backups
* export to S3 without using RCU within the PITR window
* import from S3 without WCU
* great to rapidly evolve schemas

* use case:
  * serverless apps development
  * distributed serverless cache
