# IAM

## Identity and Access Management
* users / groups / roles
* global
* Policies written in JSON
* Can have MFA
* IAM has predefined policies


- user: a phsyical person
- group: contains users
- roles: internal usage within AWS resources - we give the to machines

Each of them have _policies_ that defines what each of them can and cannot do.

- One IAM _user_ per PHYSICAL PERSON
- one IAM _role_ pero application

- User can have individual policy + group policies
  Best practice: create user > add user to group and inherit policy

## IAM Federation
* Big enterprises usually integrate their own repository of users with IAM
* Can login into AWS using their company credentials (eg: Active directory)
* Identity Federation uses the SAML standard


# EC2
* Mainly consists on the capability of:
  - renting virtual machines (EC2)
  - storing data on virtual drives (EBS)
  - distributing load across machines (ELB)
  - scaling the services using an auto-scaling group (ASG)

* AMI (Amazon Machine Image)

## Security Groups
* Firewall to EC2 instances
* Controls how traffic is allowed into and out of the EC2 Instances
* Authorized IP ranges - IPv4 and IPv6
* SGs can be attached to multiple instances
* Locked down to a Region/VPC combination
* All inbound traffic is _blocked_ by default
* SGs can have another SG inbound

## Elastic IPs
* When you stop and start an EC2 instance it changes its public IP
* If you need a public fixed IP you need and Elastic IP
* An Elastic IP is a public IPv4 IP you own _as long as you don't delete it_
* Can be attached to one instance at a time
* You can only have 5 EIPs in your account (you can ask AWS to increase that)
* Not billed as long as you use it / If EC2 instance terminated, dissasociate EIP

## User Data
* Possible to bootstrap our instances using and _EC2 User Data_ script
* Bootstrapping means launching commands when a machine starts
* Only run once when the instance starts
* Used to automate tasks:
  - install updates
  - install software
  - download common files from the internet
* Runs with the root user

## Launch Types
* On Demand Instances: short workload, predictable pricing
  - pay for what you use, billing per second after the first minute
  - highest cost but no upfront payment
  - no long term commitment
  - recommended for short therm uninterrupted workloads, where you can't predict how the app will behave
* Reserved Instances: long workload (>= 1 year)
  - up to 75% discount
  - pay upfront for what you use with long term commitmen
  - 1 to 3 year reservation period
  - reserve a specific instance type
  - recommended for steady state usage applications (database)
* Convertible Reserved Instances: long workloads with flexible instances
  - can change the EC2 isntance type
  - up to 54% discount
* Scheduled Reserved Instances: launch within window time you reserve
  - when you need it during a fraction of time and then can stop it
  - fraction of day / week / month
* Spot Instances: short workloads, very cheap, can lose instances
  - up to 90% discount compared to on-demand
  - you bid a price and get the instance as long as it's under the price
  - prices varies based on offer and demand
  - spot instances are reclaimed with a 2 minute notification warning when the spot price goes above your bid
  - used for batch jobs, big data, workload resilient to failures
  - not for critical jobs
* Dedicated Instances: no other customer will share your hardware
  - instances running on hardware dedicated to you
  - may share hardware with other instances in the same account
  - no control over instance placement
* Dedicated Hosts: book an entire physical server, control instance placement
  - physical dedicated EC2 server for your use
  - full control of the instance placement
  - visibility into the uderlying sockets / physical cores of the hardware
  - 3 year period reservation

## EC2 Instance types
* R: applications that need a lot of RAM - in-memory caches
* C: applications that need a good CPU - compute / databases
* M: applications that are balanced (medium) - general / web app
* I: applications that need a good local I/O - databases
* G: applications that need a good GPU - video rendering / machine learning
* T2/T3: burstable instances (up to a capacity)
  - burst means that overall the instance has OK CPU performance.
  - when the machine needs to process something unexpected it can burst and CPU can be VERY good
  - if it bursts, it utilizes "burst credits"
  - if all the credits are gone, CPU becomes BAD
  - when burst is gone you gain back credits
  - credits can be seen on cloudwatch
* T2/T3 unlimited: unlimited burst
  - you pay extra money if you over your credit but don't lose performance

## EC2 AMIs
* You can have custom AMIs 
* AMIs are built for a specific region - _NOT GLOBAL_
* Custom AMIs have the following advantages:
  - preinstalled packages needed
  - faster boot time (no need for EC2 user data at boot time)
  - machine comes configured with monitoring / enterprise software
  - security concerns - control over the machines in the network
  - ActiveDirectory out of the box
  - Installing your app ahead of time (faster deploys when auto-scaling)
  - Using someone else's AMI optimised for running and app, DB, etc
* You can pay for other people's AMIs
* AMIs take space and live in S3
* By default, your AMIs are private and locked for your account / region
* Can also make AMIs public and share with other AWS accounts or sell them in the marketplace
* AMIs live in S3, charged for the actual space it takes in S3
* Sharing an AMI does not affect ownership of the AMI
* If you copy and AMI that has been shared with your account, you are the owner of the target AMI in your account
* To copy an AMI that was shared with you from another account, the owner of the source AMI must grant your read permissions
  for the storage that backs the AMI, either the associated EBS snapshot (for and Amazon EBS-backed AMI) or an S3 bucket (for an instance store-backed AMI)

* Limitations:
  - You can't copy an encrypted AMI that was shared with you from another account. Instead, if the underlying snapshot and encryption key were shared with you
    you can copy the snapshot while reencypting it with a key of your own. You own the copied snapshot and can register it as a new AMI
  - You can't copy and AMI associated with a _billingProduct_ that was sahred with you from another account. Including Windows AMIs and AMIs from the Marketplace.
    To copy a shared AMI with a _billingProduct_ code, launch and EC2 instance in your account using the shared AMI and then create and AMI from the instance.


## Placement Groups
* When you want to control the EC2 Instance placement strategy
* When you create a placement group, specify one of the following strategies:
  - Cluster: cluster instances into a low-latency group in a single AZ.
    - great network (10 gbps bandwidth between instances)
    - if the rack fails, all instances fail at the same time
    - use case: big data job that need to complete fast / an app that needs extremely low latency and high network throughput
  - Spread: spreads instances across underlying hardware (max 7 instances per group per AZ) - critical applications
    - can span across AZs
    - reduced risk in simultaneous failure 
    - limited to 7 instances pero AZ per placement group
    - use case: app that needs to maximie HA / Critical apps where each instance must be isolated from failure from each other
  - Partition: spreads instances across many different partitions (which rely on different sets of racks) within an AZ. Scales to 100s of 
    EC2 instances per group (Hadoop, Cassandra, Kafka)
    - up to 7 partitions per AZ
    - up to 100s of EC2 instances
    - instances in a partition do not share racks with the instances in other partitions
    - partition failure can affect many EC2 but won't affect other partitions
    - EC2 instances get access to the partition information as metada 
    - use case: HDFS / HBase / Cassandra / Kafka




### EC2 for Solutions Architects
* Billed by the second - t2.micro is free tier
* Timeout issues => security group issues
* Permission issues => chmod 0400
* SGs can reference other SGs instead of IP ranges
* Know difference between Public, Private, Elastic IP
* You can customize EC2 instance at boot time using EC2 User Data
* 4 Launch modes:
  - on demand
  - reserved
  - spot
  - dedicated
* Instance Types: R, C, M, I, G, T2/T3
* Can create AMIs to preinstall software on your EC2 => faster boot
* AMI can be copied across regions and accounts (locked to region and account by default)
* EC2 instances can be started in placement groups:
  - cluster
  - spread
  - partition