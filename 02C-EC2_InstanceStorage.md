# EBS Volumes

* EBS (Elastic Block Store) Volume is a _network_ drive you can attach to your instances while they run
* It allows your instances to persist data
* Uses network to communicate with the instance. Might be latency
* Can be detached from the instance and attached to another one quickly as long as they are on the same AZ
* Locked to an AZ
* To move a volume you have to snapshot it first
* Have provisioned capacity (size in GBs and IOPS) => need to plan ahead.
  * you get billed by provisioned capacity not used!
* You can increaste the capacity of the drive over time
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

* st1 (HDD): Low cost HDD volume designed for frequently accesed, throughput intensive workloads
  * Streaming workloads requiring consistent, fast throughput at a low price
  * Big Data, Data Warehouses, Log processing
  * Apache Kafka
  * cannot be a boot volume
  * 500GB - 16TB
  * max IOPS is 500
  * max throughput of 500MB/S  - can burst
* sc1 (HDD): Lowest cost HDD volume designed for less frequently accesed workloads
  * throughput oriented storage for large volumes of data that is infrequently accesed
  * scenarios where lowest cost storage is important
  * cannot be a boot volume
  * 500GB to 16TB
  * max IOPS is 250
  * max throughput of 250MS/s - can burst

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

## EC2 Instance Store

* when you need high-performance hardware disk
* better I/O performance
* storage lost when instance stopped == ephemeral storage != reboot
* Instance Store
  * PROs:
    * better I/O performance
    * good for buffer / cache / scratch data / temp content
    * data survives reboots
    CONs:
    * on stop or termination instance store is lost
    * cant resize the store
    * backups operated by user
* risk of data loss if hw fails
