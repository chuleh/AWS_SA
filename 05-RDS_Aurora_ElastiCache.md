# RDS
* Managed DB service for DB use SQL as query language
* Creates dbs in the cloud that are managed by AWS
    - Postgres
    - MySQL
    - MariaDB
    - Oracle
    - Microsoft SQL Server
    - Aurora (AWS propietary database)

## RDS vs Deploying DB on EC2
* Automated provisiong (OS patching)
* Continuous backups and restore to specific timestap (Point in Time Restore)
* Monitoring dashboards
* Read Replicas for improved performance
* Multi AZ setup for DR (Disaster Recovery)
* Maintenance windows for upgrades
* Scaling capability (vertical and horizontal adding read replicas)
* Storage backed by EBS (gp2 or io1)
* CAN'T SSH INTO INSTANCES

## RDS Backups
* Automatically enabled in RDS
* Automated backups:
    - daily full backup of the database (during maintenance window)
    - transaction logs are backed-up by RDS every 5 minutes
        - ability to restore any point in time (from oldest backup to 5 minutes ago)
    - 7 days retention (can be increased to 35 days)
* DB Snapshots
    - manually trigerred by the user
    - retention of backup for as long as you want

## Read Replicas for read scalability
* Helps to scale reads
* Up to 5 Read Replicas
* Whithin AZ, Cross AZ or Cross Region
* Replications are ASYNC between main RDS instance and Read Replicas so reads are eventually consistent
    - example: if app reads from read replica before data could be replicated, might get old data
* Replicas can be promoted to be their OWN DB
* Applications must update the connection string to leverage read replicas
* Read Replicas are used for _SELECT_ only, no _INSERT_, _UPDATE_, _DELETE_ statements

### Read Replica use cases
* production db is taking on normal load
    - you want to run a reporting application to run some analytics => you create a _Read Replica_ to run the new workload there
    - prod app is unaffected

## Read Replicas - Network Cost
- Network cost when data goes from one AZ to another
- To reduce cost, you can have your Read Replicas in the same AZ

## RDS Multi AZ
* Disaster Recovery
* SYNC replication (DB in AZ A and db in AZ B - DB A writes at the same time on DB B - whic is in standby)
* One DNS name - automatic app failover to standby
* Failover in case of loss of AZ, loss of network, instance or storage failure
* No manual intervention in apps
* Not used for scaling - it's just as a failover
* Read Replicas can be setup as MultiAZ for DR

## RDS Encryption + Security
* At rest encryption
    - possibility to encrypt the master & read replicas with AWS KMS - AWS-256 Encryption
    - encryption has to be defined at launch time
        - OR unencrypted DB => snapshot => copy snapshot as encrypted => create DB from snapshot
    - if master is not encrypted, read replicas _cannot_ be encrypted
    - Transparent Data Encryption (TDE) available for Oracle and SQL Server
* In-flight encryption
    - SSL certificates to encrypt data to RDS in flight
    - Provide SSL options with trust certificate when connecting to database
    - To _enforce_ SSL:
        - PSQL: ```rds.force_ssl=1``` in the AWS RDS Console (parameter groups)
        - MySQL: Within the DB: ```GRANT USAGE ON *.* to 'mysqluser'@'%' REQUIRE SSL;```
* Network Security
    - RDS DBs usually deployed within a private subnet, not in a public one
    - RDS security works by leveraging SGs (same concept as for EC2 instances)
* Access Management
    - IAM Policies help control who can _manage_ AWS RDS (through the RDS API)
    - Traditional username and password can be used to login into the DB
    - IAM-based authentication can be used to login into RDS MySQL and PostgreSQL only
        - don't need a password, just an authentication token obtained through IAM & RDS API calls
        - auth token has a liftime of 15 minutes
        - EC2 instance will have an IAM Role > Issues API Call to get Auth Token to the RDS Service > Passes Auth Token to RDS (with SSL encryption)
    - Benefits
        - network in and out must be encrypted using SSL
        - IAM to centrally manage users instead of DB
        - can leverage IAM roles and EC2 instance profiles for easy integration

# Aurora  
* propietary technology from AWS (not open source)
* Postgres and MySQL are both supported as AuroraDB
* AWS Cloud optimized - claims 5x performance improvement over MySQL on RDS and 3x of Postgres
* Storage automatically grows in increments of 10GB up to 64TB 
* Can have 15 replicas while mysql has 5; replication process is faster (sub 10ms replica lag)
* Failover is instantaneous. It's HA native
* Costs approx 20% more than RDS
* Backtrack
    - restore data at any point of time without using backups

## High Availability and Read Scaling
* Stores 6 copies of your data across 3 AZs
    - 4 copies out of 6 needed for writes
    - 3 copies out of 6 needed for reads
    - self-healing with peer to peer replication
    - storage is striped across 100s of volumes
* One Aurora instance takes writes (master)
* Automated failover for master in less than 30 secs
* Master + up to 15 Aurora Read Replicas serve reads
* Read Replicas support cross region replication

## Aurora DB Cluster
* Shared storage volume (auto expanding from 10G to 64TB)
* Master writes to the storage
* Aurora provides a _Writer Endpoint_ pointing to the Master (a DNS Name)
* Client always talking to the writer endpoint, in failover, automatically redirected
* Read Replicas can have AutoScaling via _Reader Endpoint_
* Reader Endpoint is a Connection Load Balancing
* Reader Endpoint connects automatically to Read Replicas

## Aurora Security
* Similar to RDS as it uses the same engines
* Encryption at rest
* Automated backups, snapshots and replicas are also encrypted
* Encryption in flight using SSL (same process as MySQL or PSQL)
* Possibility to authenticate using IAM token (same as RDS)

## Aurora Serverless
* Automated database instantiaton and auto-scaling based on actual usage
* Good for infrequent, intermittent or unpredictable workloads
* No capacity planning needed
* Pay per second, can be more cost-effective
* Client connects to proxy fleet (managed by aurora) and Aurora launches instances

## Global Aurora
* Aurora Cross Region Read Replicas
    - Useful for disaster recovery
    - Simple to put in place
* Aurora Global Database (recommended by documentation)
    - 1 Primary region (read/write)
    - Up to 5 secondary (read-only) regions, replication lag is less than 1 second
    - Up to 16 Read Replicas per secondary region
    - Helps decreasing latency
    - Promoting another region (for disaster recovery) has an RTO (recovery time objective) of < 1 minute

# ElastiCache
* Managed Redis or Memcached
* Caches are in-memory databases with really hihg performance, low latency
* Helps reduced load off of databases for read intensive workloads (reads from cache, not DB)
* Helps make your application stateless
* Write Scaling using sharding
* Read Scaling using Read Replicas
* Multi AZ with failover capability
* AWS TAkes care of OS maintenance, optimizations, setup, configuration, monitoring, failure recovery and backups
* Application queries ElastiCache, if not available, get from RDS and store in ElastiCache
* Cache must have an invalidation strategy to make sure only the most current data is used in there
* User session store
    - user logs into any of the application
    - app writes the session data into ElastiCache
    - user hits another instance of our application 
    - retrieves session from elasticache and the users is already logged in

## Redis vs Memcached
* Redis
    - MultiAZ with auto-failover
    - Read Replicas to scale reads and have high availability
    - Data Durability using AOF persistence
    - Backup and restore features
* Memcached
    - multi-node for partitioning of data (sharding)
    - non-persistent cache
    - no backup and restore
    - multithreaded architecture

## ElastiCache for Solutions Architect
* Cache security
    - all caches support SSL in flight encryption
    - _do not support IAM authentication_
* Redis AUTH
    - you can set a password/token when you create a redis cluster
    - extra level of security on top of SGs
* Memcached
    - supports SASL-based authentication
* Patterns for ElastiCache
    - Lazy Loading - all the read data is cached, data can become stale in cache
    - Write Through - add or updates data in the cache when written to a DB (no stale data)
    - Session store - store temporary session data in a cache (using TTL features)
