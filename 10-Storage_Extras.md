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
  * Snowcone
  * Snowball edge
