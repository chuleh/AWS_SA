# Aurora  

* propietary technology from AWS (not open source)
* Postgres and MySQL are both supported as AuroraDB
* AWS Cloud optimized - claims 5x performance improvement over MySQL on RDS and 3x of Postgres
* Storage automatically grows in increments of 10GB up to 64TB
* Can have 15 replicas while mysql has 5; replication process is faster (sub 10ms replica lag)
* Failover is instantaneous. It's HA native
* Costs approx 20% more than RDS
* automatic fail-over
* backup and recovery
* isolation and security
* industry compliance
* push-button scaling
* automated patching with *zero* downtime
* Backtrack
  * restore data at any point of time without using backups

## Aurora HA and Read Scaling

* 6 copies of your data across 3 AZ
  * 4 copies out of 6 needed for WRITES
  * 3 copies out of 6 needed for READS
  * self-healing with peer-to-peer replication
  * storage is striped across 100s of volumes
* One Aurora instance takes WRITES (master)
* Automated failover for master in less than 30 seconds
* Master + up to 15 Aurora Read Replicas *serve reads*
* Support for Cross Region Replication

## Aurora DB Cluster

* Writer endpoint => pointing to master
* Reader Endpoint => connection load balacing between Read Replicas
* Shared storage volume (auto expanding from 10G to 64TB)
* Master writes to the storage
* Aurora provides a *Writer Endpoint* pointing to the Master (a DNS Name)
* Client always talking to the writer endpoint, in failover, automatically redirected
* Read Replicas can have AutoScaling via *Reader Endpoint*
* Reader Endpoint is a Connection Load Balancing
* Reader Endpoint connects automatically to Read Replicas
* Custom Endpoint when you have a subset of aurora reader instances larger than others
  * custom endpoints generally work having several of them and using them for different kind of queries

## Aurora Serverless

* Automated database instantiaton and auto-scaling based on actual usage
* Good for infrequent, intermittent or unpredictable workloads
* No capacity planning needed
* Pay per second, can be more cost-effective
* Client connects to proxy fleet (managed by aurora) and Aurora launches instances

## Aurora Multi-Master

* Used in case you want immediate failover in W node (HA)
* Every node does RW automatically (1x1) in case the previous one fails

## Global Aurora

* Aurora Cross Region Read Replicas
  * Useful for disaster recovery
  * Simple to put in place
* Aurora Global Database (recommended)
  * 1 primary region (rw)
  * up to secondary 5 RO regions - replication lag < 1 sec
  * up to 16 RR  per secondary regions
  * helps decreasing latency
  * promoting another region  (for DR) has an RTO of < 1m
  * typical cross-region replication takes less than 1 second

## Aurora Machine Learning

* Add ML-Based predictions to your apps via SQL
* Simple / optimized / secure integration between Aurora and AWS ML services
  * supported services
    * Amazon SageMaker (use with any ML model)
    * Amazon Comprehend (for sentiment analysis)
* You don't need ML xp
* Use cases:
  * fraud detection / ads targeting / sentiment analysis / product recommendations

## Aurora Backups

* Automated backups
  * 1 to 35 days (cannot be disabled)
  * point in time recovery in that timeframe
* Manual DB Snapshopts
  * Manually triggered by the user
  * Retention backup as long as you want

## Aurora restore options

* Restoring a snapshot creates a new DB
* Restoring MySQL Aurora cluster from S3
  * create a backup of your on-prem db using *percona xtrabackup*
  * store file on S3
  * restore file onto a new Aurora cluster running MySQL

## Aurora DB Cloning

* Allows to create a new AuroraDB from an existing one
* Faster than snapshot & restore
* New DB cluster uses the same cluster volume and data as the original but will change when data updates are made
* Very fast and cost-effective

⚠️ Useful to create a dev db from prod without impacting the prod db

## RDS Security

* At-rest encryption
  * DB master & replicas encryption using AWS KMS *must be defined at launch*
  * if master is not encrypted => RR cannot be encrypted
  ⚠️ to encrypt an un-encrypted DB => go through db snapshot and restore as encrypted
* In-flight encryption
  * TLS-ready by default - use the AWSTLS root certificates client-side
* IAM Authentication
  * IAM roles to connect to your DB (instead of user/pw)
* Security groups
  * control network access to you Aurora
* Audit logs can be enabled and sent to CloudWatch logs for longer retention
