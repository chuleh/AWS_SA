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

⚠️  Solutions Architecture: Snowball into glacier
* snowball cannot import into Glacier directly
* must use S3 first  in combination with a lifecycle policy

## Amazon FSx

* Launch 3rd party high performance file system on AWS
  * FSx for Lustre
    * parallel distributed file system for large scale computing
    * used for: Machine Learning, High Performance Computing (HPC)
    * Video Processing, Financial Modelling, Electronic Design Automation
    * scales up to 100GB/s, millions of IOPS, sub-ms latencies
    * Storage options:
      * ssd - low latency, IOPS intensive workloads, small && random file operations
      * hdd - throughput-intensive workloads, large && sequential file operations
    * Seamless integration with S3
      * can read S3 as a file system (through FSx)
      * can write the output of the computations back to S3 (thru FSx)
    * can be used from on prem (vpn, direct connect)
  * FSx for Windows File Server
    * fully managed windows filesystem shared drive
    * supports SMB protocol && Windows NTFS
    * Active Directory integration, ACLs, Quotas, etc.
    * can be mounted on Linux EC2 instances
    * Supports Microsoft's Distributed File System (DFS) namespaces (group files across multiple filesystem)
    * scale up to 10s of GB/s / millions of IOPS / 100s PB of data
    * storage options
      * SSD - latency sensitive workload (databases, media processing, data analytics)
      * HDD - broad spectrum of workloads (home directory, CMS)
    * can be accessed from on-prem infra (VPN or DirectConnect)
    * can be configured to be Multi-AZ (HA)
    * data is backed up daily to S3
  * FSx for NetApp ONTAP
    * managed NetApp ONTAP on AWS
    * File system compatible with: NFS, SMB, iSCSI protocol
    * move workloads running on ONTAP and NAS to AWS
    * works with:
      * linux
      * windows
      * macOS
      * VMWare cloud on AWS
      * AWS Workspaces && AppStream 2.0
      * Amazon EC2, EC2, EKS
    * storage shrinks or grows automatically
    * snapshots, replication, low-cost, compression and data de-duplication
    * point-in-time instantaneous cloning (helpful for testing new workloads)
  * FSx for OpenZFS
    * openZFSD file system on AWS
    * file system compatible with NFS (v1, 4, 4.1, 4.2)
    * move workloads running on ZFS to AWS
    * works with:
      * linux
      * windows
      * macOS
      * VMware Cloud on AWS
      * AWS Workspaces && AppStream 2.0
      * Amazon EC2, EC2, EKS
    * up to 1M IOPS with < 0.5 ms latency
    * snapshots, compression and low-cost
    * point-in-time instantaneous cloning (helpful for testing new workloads)
* fully managed service


## Amazon FSx File System

* Scratch file system
  * temporary storage
  * data is not replicated (doesn't persist if file server fails)
  * High burst (6x faster, 200MBPs per TiB)
  * usage: short term processing, optimize costs
* Persistent file System
  * long term storage
  * data is replicated within same AZ
  * replace failed files within minutes
  * usage: long-term processing, sensitive data

## Storage Gateway

* Hybrid cloud for storage
* bridge between on-prem data and cloud
* use cases:
  * disaster recovery
  * backup && restore
  * tiered storage
  * on-prem cache && low latency file access
* types of storage gateway
  * S3 File GW
    * create S3 file gateway
    * configured buckets are accessible using the NFS / SMB protocol
    * most recently used data is cached on the file gw
    * supports S3 standard / standard IA / One Zone / Intelligent Tiering
    * transition to Glacier using a lifecycle policy 
    * bucket access with IAM roles *for each* file Gw
    * SMB protocol has integration with AD for user auth

## Amazon FSx File Gateway

* Native access to Amazon FSx for Windows File Server
* use case:
  * local cache for frequently accessed data
  * windows native compatibility (SMB, NTFS, AD)
  * useful for group file shares and home directories

## Volume Gateway

* block storage using iSCSI protocol backed by S3
* backed by EBS snapshots which can help restore on-prem volumes
* *cached volumes* low latency access to most recent data
* *stored volumes* entire dataset is on-prem, scheduled backups to S3

## Tape Gateway

* Same process as volume but for tapes
* VTL (Virtual Tape Library) backed by S3 and Glacier
* back up data using existing tape-based processes


⚠️ for all cases, gateway *has to be installed on your datacenter* 
*However* sometimes you don't have additional virtual servers to run the gateway => *Storage Gateway Hardware appliance*

*Storage Gateway Hardware appliance*
* hardware you buy on amazon.com / has the required hardware to run the gateway client
* physical to install on your datacenter

### AWS Transfer family

* fully managed service for transferring data in and out of S3 or EFS using *ftp* protocol
* supported protocols
  * ftp (file transfer protocol)
  * ftps (file transfer protocol over SSL)
  * sftp (secure file transfer protocol)
* managed infra / scalable / reliable / HA (multi az)
* pay per provisioned endpoint per hour + data transfers in GB
* store and manage users credentials within the service
* integrates with:
  * AD - LDAP - Okta AWS Cognito - custom

### AWS DataSync

* move large amount of data to and from 
  * on-prem / other cloud to AWS (NFS, SMB, HDFS, S3 API, etc) - needs agent
  * AWS to AWS - no agent
* can synchronize to:
  * s3
  * EFS
  * FSx
* replication tasks can be schedule: hourly, daily, weekly
* file permissions and metadata are preserved (NFS POSIX, SMB)
* one agent task can use 10gbp - can setup a bandwidth limit

⚠️  when not enough bandwidth use *snowcone*
