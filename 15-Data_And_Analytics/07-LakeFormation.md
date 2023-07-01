# LakeFormation

* helps create Data Lakes
* data lake == central place to have all your data for analytics purposes
* fully managed service that makes it easy to set up data lakes in *days* (can take months without lakeformation)
* discover, cleanse, transform and ingest data into your data lake
* it automates many complex manual steps (collectiong, cleansing, moving, cataloging data) and de-duplicate (using ML transforms)
* combine structured and unstructured data in the data lake
* out-of-the-box source blueprints: S3, RDS, Relational && NoSQL DBs
* fine-grained access control for your applications: row and column level
* layer built on top of Glue

## Structure

* Data sources
  * S3
  * RDS
  * Aurora
  * on-prem
* ingest data with blueprints available onto LakeFormation
  * lakeformation feats:
    * source crawlers
    * ETL and data prep
    * data catalog
    * security settings
    * ACL
* data leveraged by
  * Athena
  * RedShift
  * EMR
  * Apache Spark
* users start here and goes up

## LakeFormation Centralized permissions example

* Several datasources: RDS, S3, Aurora
* data analyzed with: Athena && QuickSight
* permissions go where? bucket level? user level? quicksight level? athena level? == mess
* lakeformation solves it by giving ACL on column-row level security => so services connecting to LakeFormation will only see what's allowed there
