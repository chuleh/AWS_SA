# Database Migration Service

* quickly and secure migrate DBs to AWS
* secure, resilient && self-healing
* source DB remains available during migration
* supports:
  * homogeneous migrations == oracle to oracle
  * heterogeneous migrations == MsSQL to Aurora
* continuous data replication using CDC (change data capture)
* must create EC2 instance that'll run the DMS (datacenter => EC2 with DMS => target DB)

## Sources && Targets

* sources:
  * on-prem && EC2 dbs
    * oracle, mssql, mysql, mariadb, psql, mongodb, sap, db2
  * azure sql db
  * amazon RDS (including aurora)
  * S3
  * DocumentDB
* targets
  * on-prem && EC2 dbs
    * oracle, mssql, mysql, mariadb, psql, SAP
  * rds
  * redshift, dynamodb, S3
  * OpenSearch Service
  * Kinesis Data Streams
  * Kafka
  * DocumentDB && Neptune
  * Redis && Babelfish

## AWS SCT - Schema Conversion Tool

* convert schema from one engine to another
* ex: OLTP (SQL Server or Oracle) => MySQL, Psql, Aurora
* ex: OLAP (Teradata or Oracle) => Redshift
* no SCT if same engine (including on-prem and aws)

## DMS - Continuous Replication

* Datacenter with DB => on-prem server with SCT
  * data migration to AWS DMS repl instance => full load + cdc => RDS MySQL
    * DMS repl instance on pub subnet => RDS on priv sub
  * schema conversion => AWS RDS for MySQL (target)

## DMS - MultiAZ deployment

* when enabled
  * multiaz DMS provisions and maintains a synchonously stand replica in a diff AZ
* provides data redundancy
* eliminates I/O freezes
