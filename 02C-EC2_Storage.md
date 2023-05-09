# EBS Volumes

* EBS (Elastic Block Store) Volume is a _network_ drive you can attach to your instances while they run
* It allows your instances to persist data
* Uses network to communicate with the instance. Might be latency
* Can be detached from the instance and attached to another one quickly as long as they are on the same AZ
* Locked to an AZ
* To move a volume you have to snapshot it first
* Have provisioned capacity (size in GBs and IOPS) => need to plan ahead.
  * you get billed by provisioned capacity not used!
* You can increase the capacity of the drive over time
* Only IO1 and GP2 can be used as boot volumes!
* think of like a "network usb stick"
* one or more EBS volumes can be attached to an instance
* EBS volumes can be detached and left detached with the stored data.
* Attach on-demand.

⚠️ delete on terminaton box => unchecked by default.

⚠️ controls the EBS volume behaviour when an EC2 instance terminates

⚠️ enabled for root volume

## EBS Snapshots

* Backup of your EBS volume at a point in time
* Incremental - only backup changed blocks
* EBS backups use IO and shouldn't be run while app is handling a lot of traffic
* snapshots stored in S3 but you don't directly see them
* not necessary to detach volume to do snapshot but recommended
* max 100k snapshots
* can copy  snapshots across AZ or Region
* can make AMI from a snapshot
* EBS volumes restored by snapshots need to be prewarmed (using fio or dd to read the entire volume)
* Snapshots can be automated using Amazon Data Lifecycle Manager

⚠️ EBS Snapshot Archive => move snapshot to "archive tier" => 75% cheaper

    - Takes from 24 to 72hs to be retrieved when archived

⚠️ EBS Recycle bin for EBS Snapshots

    - setup rules to retain deleted snapshots so they can be recovered even after deletion
    - retention from 1 day to 1 year️

⚠️ Fast snapshot restore  (FSR)

    - force full initialization of the snapshot to have no latency on the first use
    - very expensive

## EBS Volume Types

* ⚠️ Only GP2/GP3 io1/io2 can be used as boot volumes

* GP2/GP3 (SSD): General Purpose SSD - balance price and performance for a wide variety of workloads == cost effective
* range from 1G to 16TB
  * gp3
    * Baseline of 3000 IOPS and throughout of 125mb/s
    * can increase IOPS to 16K and throughput to 1K mb/s independently
  * gp2
    * small gp2 volumes can burst up to 3K IOPS
    * size of the volume and IOPS are linked. max IOPS is 16K
    * 3 IOPS per GB means at 5334GB we are at the max IOPS
* io1/io2 (SSD): Highest performance SSD volume for a mission-critical low-latency or high-throughput workloads (Critical DB) == PIOPS (Provisioned IOPS)
  * critical business applications that require sustained IOPS performance or more than 16000 IOPS per volume (gp2 limit)
  * Large database workloads: mongdb, cassandra, MSSQL, MySQL, PGSQL, Oracle
  * 4GB to 16Tb
  * IOPS is provisioned (PIOPS) - MIN 100 / MAX 64000 (EC2 nitro instances) else MAX 32000 (other instances)
  * Maximum ratio of provisioned IOPS to requested volume size (in GB) is 50:1 - 50IOPS each 1 GB
  * Can increase PIOPS independently from storage size

  * io2
    * io2 has more more durability and more IOPS per GB (at the same price of io1)
    * io2 Block Express => (4gb - 16tb)
    * sub-milisecond latency
    * MAX PIOPS: 256K with an IOPS:GB ratio of 1000:1
    * supports EBS multi-attach

* HDD (Hard Disk Drives)
* st1 (HDD): Low cost HDD volume designed for frequently accesed, throughput intensive workloads
  * throughput optimized
  * cannot be a boot volume
  * Streaming workloads requiring consistent, fast throughput at a low price
  * Big Data, Data Warehouses, Log processing
  * Apache Kafka
  * 500GB - 16TB
  * max IOPS is 500
  * max throughput of 500MB/S  - can burst
* sc1 (HDD): Lowest cost HDD volume designed for less frequently accesed workloads
  * cannot be a boot volume
  * Cold HDD
  * infrequently accessed data
  * throughput oriented storage for large volumes of data that is infrequently accesed
  * scenarios where lowest cost storage is important
  * 500GB to 16TB
  * max IOPS is 250
  * max throughput of 250MS/s - can burst

## EBS Multi-Attach

* io1/io2 family
* attach the same EBS volume to multiple EC2 instances in the same AZ
* each instance has RW permission to the high-performance volume
* Use case:
  * higher application availability in clustered linux apps (ex: Teradata)
  * apps must manage concurrent write operations
  * cannot attach from one AZ to another AZ
  * up to 16 EC2 instances at a time
  * must use a file system that is cluster-aware (not XFS, EXT4, etc)

## EBS Encryption

* data at rest is encrypted inside the volume
* data in flight moving between instance and volume is encrypted
* all snapshot are encrypted
* all volumes created from snapshot are encrypted
* encryption and decryption are handled transparently by EC2 and EBS
* minimal impact on latency
* EBS encryption leverages keys from KMS (AES-256)
* Copying and unencrypted snapshot allows encryption
* snapshots of encrypted volumes are encrypted
* Encrypt an unencrypted EBS volume
  * create a snapshot of the volume
  * encrypt the EBS snapshot (using copy) -- copy the snapshotted volume and create a copy of it and then you can
    encrypt it
  * create a new EBS volume from the snapshot (the volume will also be encrypted)
  * now you can attach the volume to the original instance

## EBS Volume migration

* EBS Volumes locked to a specific AZ
* To migrate to a different AZ or Region
  * snapshot the volume
  * copy the volume to a different region
  * create a volume from the snapshot in the AZ of your choice

## EBS RAID Options

* RAID possible as long as OS supports it
* RAID 0
  * 1 logical volume
  * striped
  * increase performance
  * Combining 2 or more volumes and getting the total disk space and I/O
  * One disk fails, all data is failed
  * Use cases:
    * app that needs lots of IOPS and doesn't need fault tolerance
    * DB that already has replication built-in
  * Using this we can have a very big disk with a lot of IOPS
    * eg: 2 500GB EBS io1 Volumes with 4k provisioned IOPS each will create 1k GB RAID 0 Array with bandwidth of 8k IOPS
      and 1k MB/s of throughput
* RAID 1
  * 1 logical volume
  * mirored
  * fault tolerance
  * if one disk fails our logical volume is still working
  * more network throughput > send data to 2 EBS volumes at the same time (2x network)
  * Use cases:
    * app that need increase fault tolerance
    * need to service disks
* RAID 5 (not recommended for EBS)
* RAID 6 (not recommended for EBS)

## EFS  Elastic File System

* Managed NFS (network file system) that can be mounted on many EC2
* Same region
* Multi AZ
* Higly available
* Scalable
* Expensive (3x gp2)
* _pay per use_
* Needs an SG
* Uses NFS v4.1 protocol
* use cases: content management / web serving / data sharing / wordpress
* Linux only
* Performance mode:
  * general purpose (default)
  * Max I/O - used when thousands of EC2 are using the same EFS
* EFS file sync to sync from on-prem file system to EFS
* Backup EFS-to-EFS (incremental - can choose frequency)
* Encryption at rest using KMS

## EFS Performance and storage

* EFS Scale
  * 1000s of concurrent NFS clients 10GB+/s throughput
  * grow to _Petabyte_ scale network file system _automatically_

* Performance mode (set at EFS creation time)
  * General Purpose (default): latency-sensitive use cases (web server, cms, etc)
  * Max I/O:  higher latency, throughput, higly parallel (big data, media processing)

* Throughput mode
  * Bursting (1 TB = 50Mb/s + burst of up to 100 MB/s)
  * Provisioned: set your throughput regardless of storage size
    * ex: 1Gb/s for 1Tb storage

* EFS Storage Classes
  * Storage Tiers (lifecycle management feature - move file after X days)
    * Standard: for frequently accessed files
    * Infrequent Access (EFS-IA): cost to retrieve files - lower price to store
      enable EFS-IA with a Lifecycle Policy
  * Availability and durability
    * Regional: Multi-AZ, great for prod  (_standard_ is the old name)
    * One Zone: One AZ, great for dev, backup enabled by default, compatible with IA (EFS one-zone IA)
    * over 90% cost in savings

### EBS vs EFS

* EBS Volumes:
  * can be attached to only one instance at a time
  * are locked at the AZ level
  * gp2: IO increases if the disk size increases
  * io1: can increase IO independently
  * to migrate an EBS volume across AZ
    * take a snapshot
    * restore snapshot to another AZ
  * EBS backups use IO and _you shouldn't run them_ while your app is handling a lot of traffic
  * Root EBS Volumes of instances get terminated by default if the EC2 instance gets terminated
    (can be disabled)
  * pay for provisioned capacity

* EFS
  * can be mounted 100s of instances across AZ
  * Linux only
  * used to share website files (wordpress ex)
  * higher price point than EBS
  * can leverage EFS-IA for cost savings
  * billed only for what you use

## EC2 Instance Store

* when you need high-performance hardware disk
* better I/O performance
* storage lost when instance stopped == ephemeral storage != reboot
* Instance Store
  * PROs:
    * better I/O performance
    * good for buffer / cache / scratch data / temp content
    * data survives reboots
  * CONs:
    * on stop or termination instance store is lost
    * can't resize the store
    * backups operated by user
* risk of data loss if hw fails

## AMI Overview

* AMI == Amazon Machine Image
* customization of an EC2 instance
  * you add your own software / config / OS / monitoring / etc
  * faster boot / config => all previous software added
  * AMI built for specific region _but_ can be copied across regions
* you can launch EC2 instances from:
  * public AMI => aws provided
  * your own AMI => make them and maintain yourself
  * AWS marketplace AMI => made and probably sold by someone else
* AMIs locked to region > copy your AMI from Region 1 to Region 2 and launch

### Wrap up

* EFS: for network file sharing - mounted across multiple instances - several AZ
* EBS: for a network volume attached to one instance - same AZ
* Instance Store: maximum IO on an EC2 instance - ephemeral
