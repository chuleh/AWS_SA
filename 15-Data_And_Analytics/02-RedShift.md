# RedShift

* based on PSQL but *not used* for *OLTP*
* it's *OLAP* online analytical processing (analytics and data warehousing)
* 10x better performance than other data warehouses, scales to PB of data
* columnar storage data (instead of row based) && parallel query engine
* pay as you go on the instances provisioned on your redshift cluster
* has an SQL interface for performing queries
* BI tools such as Amazon Quicksight or Tableau integrate with it
* vs Athena: faster queries / joins / aggregations thanks to indexes

## RedShift Cluster

* Leader node: for query planning, results aggregation
* compute node: for performing the queries, sends results to leader
* provision the node size in advance
* can use reserved instances for cost saving

## Snapshots && DR

* multi-az mode for some clusters
* snapshots are PITR backups of a cluster stored internally in S3
* snapshots are incremental
* can restore snapshot into a new cluster
* automated: every 8 hours / every 5GB or on a schedule
  * can set retention
* manual: snapshot is retained until you delete it
* can configure RedShift to automatically copy snapshots (automated or manual) of a cluster to another AWS region

## Loading data into RedShift

* larger inserts are *much better*
* Amazon Kinesis Data Firehose:
  * recieves data from diff sources
  * writes data into S3 bucket
  * inserts it into RedShift cluster (through S3 copy)
* S3 using *copy* command:
  * load data into S3
  * issue copy command directly from RedShift
  * uses IAM roleo
  * traffic can go public (without enhanced vpc routing)
  * through vpc (internal, with enhanced vpc routing)
* EC2 instance with JDBC driver
  * from EC2 to RedShift with an app
  * better to write data in batches

## RedShift Spectrum

* query data already in S3 without loading it
* *must have* a redshift cluster available to start the query
* the query is then submitted to thousands of RedShift Spectrum nodes
* data gets sent back to RedShift cluster
