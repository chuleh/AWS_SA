# Amazon Aurora

## Summary

* compatible API for PSQL / MySQL, separation of storage and compute
* storage: data is stored in 6 replicas, across 3 AZ - highly available, self-healing, auto-scaling
* compute: cluster of DB instance across multiple AZ, auto-scaling of Read Replicas
* cluster: custom endpoints for writer and reader DB instances
* same security / monitoring / maintenance features as RDS
* *need* to know backup & restore options for Aurora

* options:
  * aurora serverless: for unpredictable / intermittent workloads, no capacity planning
  * aurora multi-master: for continuous writes failover (high write availability)
  * aurora global: up to 16 DB Read Instances in each region. < 1 second storage replication
  * aurora machine learning: perform ML using SageMaker & Comprehend on Aurora
  * aurora database cloning: create new cluster from existing one, faster than restoring a snapshot

* use case:
  * same as RDS but less maintenance, more flexibility, more performance, more features
