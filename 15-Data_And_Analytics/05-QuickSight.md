# QuickSight

* serverless machine-learning powered business intelligence service to create interactive dashboards
* fast, auto-scalable, embeddable with per sessions pricing

* use cases:
  * business analytics
  * building visualizations
  * perform ad-hoc analysis
  * get business insights using data
* integrated with:
  * AWS
    * RDS
    * Aurora
    * Athena
    * S3
    * RedShift
  * 3rd party == SaaS
    * SalesForce
    * Jira
  * 3rd party DBs
    * Teradata
    * on-prem DBs using JDBC
  * imports
    * XLSX
    * CSV
    * JSON
    * TSV
    * ELF && CLF (log format)
* in-memory computation using *SPICE* engine if data is imported into QuickSight
* Enterprise Edition: possibility to set up *Column-Level Security* (CLS)

## Dashboard && Analysis

* define Users (standard versions) and Groups (enterprise versions)
  * these users & groups *only exist in QuickSight NOT IAM*
* create a dashboard
  * dashboard is a RO snapshot of an analysis that you can share
  * preserves the configuration of the analysis (filtering, parameters, control, sort)
* can share the dashboard with either users or groups
* must first publish dashboard before sharing
* users who see dashboard can also see the underlying data
