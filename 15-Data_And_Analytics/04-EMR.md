# EMR

* Elastic MapReduce
* EMR helps creating Hadoop Clusters (BigData) to analyze and process vast amount of data
* clusters can be made of hundreds of EC2 instances
* comes bundled with: Spark, HBase, Presto, Flink
* difficult to set up - EMR takes care of provisioning and configuration
* autoscaling and integrated with spot instances

* use cases:
  * data processing
  * machine learning
  * web indexing
  * big data

## Node Types && purchasing

* Master Node: manages the cluster, coordinates, manages health - long running
* Core Node: run tasks and store data - long running
* Task Node (optional): just to run tasks - usually Spot

* purchasing options:
  * on-demand: reliable, predictable, won't be terminated
  * reserved: 1 year min, cost savings, EMR will automatically use if available
  * spot: cheaper, can be terminated, less reliable

* can have long-running clusters or *transient* (temporary clusters)
