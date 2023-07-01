# Glue

* managed ETL service (Extract, Transform, Load)
* useful to prepare and trasnform for data analytics
* fully *serverless*

* use case
  * load data into RedShift
    * data in RDS or S3
    * sent to glue => extracted => trasnformed => loaded onto RedShift
  * convert data into Parquet format
    * PUT data into S3 (maybe CSV)
    * import CSV into Glue
    * Glue transforms to Parquet
    * PUTS on another S3 bucket
    * analyzed by Athena
    ⚠️ can add *event notification* on S3 PUT for
      * lambda that triggers Glue
      * or EvenBridge that triggers Glue

## Glue Data Catalog

* AWS Glue Data Crawler gathers data crawling from
  * S3
  * RDS
  * DynamoDB
  * JDBC
* Data Crawler writes metadata onto *AWS Glue Data Catalog*
  * will have all the DBs and tables with metadata
  * leveraged by Glue jobs to perform ETL
* Data discovery done by either
  * Athena
  * RedShift Spectrum
  * EMR
  * all leverage Glue Data Catalog

## Good things to know

* Glue Job Bookmarks: prevents re-processing old ata
* Glue Elastic views: combine and replicate data across multiple data stores using SQL
  * no custom code - glue monitors for changes in the source data
  * serverless
  * leverages a virtual table (materialized view)
* Glue Databrew: clean and normalize data using pre-built trasnformation
* Glue Studio: new GUI to create, run and monitor ETL jobs in Glue
* Glue Streaming ETL: (built on Apache Spark Structured streaimng):
  * instead of running ETL as batch can be run as streaming jobs
  * can read data from Kinesis Data Streams, Kafka, MSK
