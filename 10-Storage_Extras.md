# AWS Storage Extras

## Snow Family

* Offline devices to perform data migration
* Highly secure portable devices to collect and process at the edge and migrate data into and out of AWS
* Aims to solve challenges
  * limited connectivy
  * limited bandwidth
  * high network cost
  * shared bandwidth
  * connection stability
* Data Migration
  * Snowball Edge
    * moves TBs or PBs of data in and out of AWS
    * pay per data transfer job
    * provide block storage and Amazon S3 compatible object storage
    * 2 types
      * Snowball edge storage optimized: *80 TB of HDD capacity* for block volume and s3 compatible object storage
      * Snowball edge compute optimized: *42 TB of HDD capacity* for block volume and s3 compatible object storage
  * Snowcone
    * small, portable computing anywhere, rugged, secure, withstands harsh environments
    * light 
    * used for: edge computing, storage, data transfer
    * *8 TB* of usable storage
    * used when snowball doesn't fit
    * must provide own battery / cables
    * can be sent back to AWS offline or connect it to inet and use *AWS DataSync* to send Data
  * Snowmobile
    * truck to transfer exabytes of data (1M TB)
    * each snowmobile has 100 PB of capacity (use multiple in parallel)
    * high security: temperature controlled, GPS, 24/7 video surveillance
    * if > 10 TB of data == use snowmobile
* Edge Computing
  * process data while it's being created on an *edge location*
    * edge location: limited / no internet access - limited / no easy access to computing power
    * truck on the road / ship on the sea / mining station underground
  * Snowcone
    * 2 CPUs / 4G of memory / wire - wireless access
    * USB-C power cord or optional battery
  * Snowball edge - compute optmized
    * 52 vCPUs / 208G of RAM
    * optinal GPU
    * 42 TB
  * Snowball edge - Storage optimized
   * up to 40 vCPUs / 80G of RAM
   * object storage clustering available
  * preprocess data
  * machine learning at the edge
  * transcoding media streams
  * eventually (if needed) can ship back device to AWS
  * *ALL* can run EC2 instances && AWS Lambda Functions
  * long term discounts for 1-3 years

* AWS OpsHub
  * SW you install on your computer to manage snow family device
  * unlocking and configurin single or clustered devices
  * launch / manage instances running on Snow devices
  * monitor device metrics
  * launch compatible AWS services on device: EC2 / DataSync / NFS
