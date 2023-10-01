# RDS && Aurora MySQL Migrations

* RDS MySQL to Aurora MySQL migrations
  * options
    * snapshots from rds mysql restored as mysql aurora db
      * downtime
    * create Aurora Read Replica from RDS
      * when replication lag is 0 => promote it as its own DB cluster (can take time && cost)
* External MySQL to Aurora MySQL
  * options
    * Percona Xtrabackup to create a file backup in S3
      * create Aurora MySQL DB from S3
    * create aurora MySQL DB
      * mysql dump and pipe into Aurora
      * slower than S3
    * DMS if both DBs are running

## RDS && Aurora PostgreSQL Migrations

* RDS PSQL to Aurora PSQL
  * same as above but for PSQL
* External PSQL to Aurora PSQL
  * create backup && PUT in S3
  * import using *aws_s3* Aurora extension
* DMS if both running
