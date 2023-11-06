# High Performace Computing

* cloud
  * can create high number of resources in no time
  * can speed up time to results by adding more resources
  * pay only for the systems you used
* what for
  * genomics, computational chemistry, financial risk modeling, weather prediction, machine learning, deep learning, autonomous driving
* services that perform HPC
  * Data Mgmt && Transfer
    * AWS Direct Connect
      * move GB/s of data to the cloud over a private secure network
    * Snowball && Snowmobile
      * move PB of data to the cloud
    * AWS DataSync
      * large amount of data between on-premise and S3, EFS, FSx for Windows
  * Compute and networking
    * EC2 instances
      * CPU/GPU optimized
      * spot instances/spot fleets for cost savings + autoscaling
    * EC2 placement groups
      * cluster type
      * good for network performance (same rack, same az)
    * networking
      * EC2 enhanced networking (SR-IOV)
        * higher bandwidth
        * higher PPS (packet per second)
        * lower latency

