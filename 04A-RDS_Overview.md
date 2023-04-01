
# RDS

* Managed DB service for DB use SQL as query language
* Creates dbs in the cloud that are managed by AWS
  * Postgres
  * MySQL
  * MariaDB
  * Oracle
  * Microsoft SQL Server
  * Aurora (AWS propietary database)

## RDS vs Deploying DB on EC2

* *Managed Service*
  * Automated provisioning (OS patching)
  * Continuous backups and restore to specific timestamp (Point in Time Restore)
  * Monitoring dashboards
  * Read Replicas for improved performance
  * Multi AZ setup for DR (Disaster Recovery)
  * Maintenance windows for upgrades
  * Scaling capability (vertical and horizontal adding read replicas)
  * Storage backed by EBS (gp2 or io1)
* CAN'T SSH INTO INSTANCES

## RDS - Storage Auto Scaling

* Increases storage on your RDS DB instance dynamically
* Triggers when RDS detects your are running out of free DB storage => scales automatically
  * You need to set *Maximum Storage Threshold*
  * Automatically modify storage if:
    * free storage is less than 10% of allocated storage
    * low storage lasts at least 5 minutes
    * 6 hours have passed since last modification
* Useful for apps with *unpredictable workloads*
* Supports all RDS DB Engines

## RDS - Read Replicas

* Helps to scale reads
* Up to 5 Read Replicas
* Whithin AZ, Cross AZ or Cross Region
* Replications are *ASYNC* between main RDS instance and Read Replicas so reads are eventually consistent
  * example: if app reads from read replica before data could be replicated, might get old data
* Replicas can be promoted to be their OWN DB
* Applications must update the connection string to leverage read replicas
* Read Replicas are used for *SELECT* only, no *INSERT*, *UPDATE*, *DELETE* statements
* Read Replica use cases
  * production db is taking on normal load
    * you want to run a reporting application to run some analytics => you create a *Read Replica* to run the new workload there
    * prod app is unaffected
* Read Replica network cost
  * Network cost when data goes from one AZ to another
  * Read Replicas whithin same region you don't pay that fee

## RDS Multi AZ

* Disaster Recovery
* SYNC replication (DB in AZ A and db in AZ B - DB A writes at the same time on DB B - which is in standby)
* One DNS name - automatic app failover to standby DB => increase availability
* Failover in case of loss of AZ, loss of network, instance or storage failure
* No manual intervention in apps
* Not used for scaling - it's just as a failover
* Read Replicas can be setup as MultiAZ for DR
* RDS - from single-az to multi-az
  * Zero downtime operation (no need to stop the DB)
    * Click on *modify* for the db
    * automatically snapshot is taken
    * a new DB is restored from the snapshot in a new AZ
    * synchronization is established between the two databases

## RDS Custom

* Managed Oracle and MS SQL Server database *with* OS and DB Customization
* RDS still automates setup, operation and scaling of DB in AWS
* *Custom*: access to underlying database and OS
  * you can configure settings
  * install patches
  * enable native features
  * *access* the underlying EC2 instance using SSH or SSM Session Manager
* *Recommended* to deactivate automation mode to perform customization => better to take a snapshot before

## RDS Backups

* Automatically enabled in RDS
* Automated backups:
  * daily full backup of the database (during maintenance window)
  * transaction logs are backed-up by RDS every 5 minutes
    * ability to restore any point in time (from oldest backup to 5 minutes ago)
  * 7 days retention (can be increased to 35 days)
* DB Snapshots
  * manually triggered by the user
  * retention of backup for as long as you want

⚠️  in a stopped RDS DB you will still pay for storage. If you plan to stop for a long time => snapshot and restore instead.

## RDS Restore options

* Restoring a snapshot creates a new DB
* Restoring MySQL RDS database from S3
  * create a backup of your on-prem db
  * store it on S3
  * restore the backup file onto a new RDS instance running mysql

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
  * control network access to you RDS
* *NO SSH* except on RDS Custom
* Audit logs can be enabled and sent to CloudWatch logs for longer retention
