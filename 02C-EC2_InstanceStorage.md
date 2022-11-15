# EBS Volumes
* EBS (Elastic Block Store) Volume is a _network_ drive you can attach to your instances while they run
* It allows your instances to persist data
* Uses network to communicate with the instance. Might be latency
* Can be detached from the instance and attached to another one quickly as long as they are on the same AZ
* Locked to an AZ
* To move a volume you have to snapshot it first
* Have provisioned capacity (size in GBs and IOPS) => need to plan ahead.
    - you get billed by provisioned capacity not used!
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

## AMI Overview