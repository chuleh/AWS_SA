# EBS Volumes
* EBS (Elastic Block Store) Volume is a _network_ drive you can attach to your instances while they run
* It allows your instances to persist data
* Uses network to communicate with the instance. Might be latency
* Can be detached from the instance and attached to another one quickly as long as they are on the same AZ
* Locked to an AZ
* To move a volume you have to snapshot it first
* Have provisioned capacity (size in GBs and IOPS)
  - you get billed by provisioned capacity not used!
* You can increaste the capacity of the drive over time
* Only IO1 and GP2 can be used as boot volumes!

## EBS Volume Types
* GP2 (SSD): General Purpose SSD - balance price and performance for a wide variety of workloads
  - recommended for most workloads
  - system boot volumes
  - 1GB to 16TB
  - small gp2 volumes can burst IOPS to 3000
  - MAX IOPS 16000 > 3 IOPS per GB, means 5334 we are at the max IOPS
* IO1 (SSD): Highest performance SSD volume for a mission-critical low-latency or high-throughput workloads (Critical DB)
  - critical business applications that require sustained IOPS performance or more than 16000 IOPS per volume (gp2 limit)
  - Large database workloads: mongdb, cassandra, MSSQL, MySQL, PGSQL, Oracle
  - 4GB to 15Tb
  - IOPS is provisioned (PIOPS) - MIN 100 - MAX 64000 (nitro instances) else MAX 32000 (other instances)
  - Maximum ratio of provisioned IOPS to requested volume size (in GB) is 50:1 - 50IOPS each 1 GB
* ST1 (HDD): Low cost HDD volume designed for frequently accesed, throughput intensive workloads
  - Streaming workloads requiring consisten, fast throughput at a low price
  - Big Data, Data Warehouses, Log processing
  - Apache Kafka
  - cannot be a boot volume
  - 500GB - 16TB
  - max IOPS is 500
  - max throughput of 500MB/S  - can burst
* SC1 (HDD): Lowest cost HDD volume designed for less frequently accesed workloads
  - throughput oriented storage for large volumes of data that is infrequently accesed
  - scenarios where lowest cost storage is important
  - cannot be a boot volume
  - 500GB to 16TB
  - max IOPS is 250
  - max throughput of 250MS/s - can burst

## EBS Snapshots
  * Incremental - only backup changed blocks
* EBS backups use IO and shouldn't be run while app is handling a lot of traffic
* snapshots stored in S3 but you don't directly see them
* not necessary to detach volume to do snapshot but recommended
* max 100k snapshots
* can copy  snapshots across AZ or Region
* can make AMI from a snapshot
* EBS volumes restored by snapshots need to be prewarmed (using fio or dd to read the entire volume)
* Snapshots can be automated using Amazon Data Lifecycle Manager

## EBS Volume migration
* EBS Volumes locked to a specific AZ
* To migrate to a different AZ or Region
  - snapshot the volume
  - copy the volume to a different region
  - create a volume from the snapshot in the AZ of your choice

## EBS Encryption
* data a rest is encrypted inside the volume
* data in flight moving between instance and volume is encrypted
* all snapshot are encrypted
* all volumes created from snapshot are encrypted
* encryption and decryption are handled transparently by EC2 and EBS
* minimal impact on latency
* EBS encryption leverages keys from KMS (AES-256)
* Copying and unencrypted snapshot allows encryption
* snapshots of encrypted volumes are encrypted
* Encrypt an unencrypted EBS volume
  - create a snapshot of the volume
  - encrypt the EBS snapshot (using copy) -- copy the snapshotted volume and create a copy of it and then you can
    encrypt it
  - create a new EBS volume from the snapshot (the volume will also be encrypted)
  - now you can attach the volume to the original instance

## EBS vs Instance Store
* Some instances do not come with Root EBS Volumes > come with "instance store" (ephemeral storage)
* Instance store attached physically to machine whereas EBS is a network drive
* Instance Store 
    - PROs:
      - better I/O performance
      - good for buffer / cache / scratch data / temp content
      - data survives reboots
    CONs:
      - on stop or termination instance store is lost
      - cant resize the store
      - backups operated by user

## EBS RAID Options
* RAID possible as long as OS supports it
* RAID 0
  - 1 logical volume
  - striped
  - increase performance
  - Combining 2 or more volumes and getting the total disk space and I/O
  - One disk fails, all data is failed
  - Use cases:
    - app that needs lots of IOPS and doesn't need fault tolerance
    - DB that already has replication built-in
  - Using this we can have a very big disk with a lot of IOPS
    - eg: 2 500GB EBS io1 Volumes with 4k provisioned IOPS each will create 1k GB RAID 0 Array with bandwidth of 8k IOPS
      and 1k MB/s of throughput
* RAID 1
  - 1 logical volume
  - mirored
  - fault tolerance
  - if one disk fails our logical volume is still working
  - more network throughput > send data to 2 EBS volumes at the same time (2x network)
  - Use cases:
    - app that need increase fault tolerance
    - need to service disks
* RAID 5 (not recommended for EBS)
* RAID 6 (not recommended for EBS)

# EFS  Elastic File System
* Managed NFS (network file system) that can be mounted on many EC2
* Same region
* Multi AZ
* Higly available / scalable / expensive (3x gp2) / pay per use
* Needs an SG
* Uses NFS v4.1 protocol
* use cases: content management / web serving / data sharing / wordpress
* Linux only
* Performance mode:
  - general purpose (default)
  - Max I/O - used when thousands of EC2 are using the same EFS
* EFS file sync to sync from on-prem file system to EFS
* Backup EFS-to-EFS (incremental - can choose frequency)
* Encryption at rest using KMS

## EBS EFS for Solutions Architect
* EBS volumes can be attached to onlye one instance at time
* EBS volumes are locked at the AZ level
* Migrating an EBS volume across AZ means firt backing it up (snapshot) then recreating it in the other AZ
* EBS backups use I/O and shouldn't be run while the app is handling a lot of traffic
* Root EBS volumes of instances get terminated by default if the EC2 instance gets terminated (can be disabled)
* Disk IO is high > Increase EBS volume size (for gp2)
* EFS attached to many instances over many AZs
* EFS share website files
* EBS gp2 optimize on cost
* EBS > Custom AMI for faster deploy on ASG
* Instance Store: actual volume on the instance itself (ephemeral storage, cannot be attached or detached, lost on
  termination or stop)

