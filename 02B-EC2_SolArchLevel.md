# EC2 Solutions Architect Associate Level

## Private IP vs Elastic IP vs Public IP

- Public IP
  - means the machine can be identified on the internet
  - must be unique across the whole web (no two machines can have the same Public IP)
  - can be geo-located easily
- Private IP
  - means the machine can be identified on a local network only
  - the IP must be unique across the private network
  - two different private networks can have the same IP
  - machines connect to the web using a NAT Gateway
  - only a specified range of IPs can be used as private IPs
- Elastic IP
  - when you stop/start an EC2 instance it can change its public IP
  - if you need to have a fixed *public* IP for your instance you need an Elastic IP
  - Elastic IP is a public IPv4 IP you own as long as you don't delete it
  - can be attached to one instance at a time
  - useful for masking failure of instance/app by moving the Elastic IP to another instance
  - you can have only 5 EIPs in your AWS account (can ask AWS to increase it)
- ProTip:
  - avoid using EIPs
    - reflects poor architectural decisions
    - instead, use random public IP and register a DNS name to it
    - use a Load Balancer and don't use a Public IP at all

## EC2 Placement Groups

- control over the EC2 placement strategy
- strategies:
  - *Cluster*:
    - cluster instances into a low-latency gorup in a single AZ
    - same rack - same AZ
    - low latency - 10Gbps bandwidth between instances
    - if rack fails == all instance fails
    - use case:
      - Big Data job that needs to complete fast
      - app that needs extreme low latency and high network throughput
  - *Spread*:
    - spread instances across underlying hardware (max 7 instances per group per AZ)
    - spans across different AZs
    - reduced risk of failure
    - EC2 instances are on different hardware
    - limited to 7 instances per placement group
    - use case:
      - app that needs to maximize high availability
      - critical app where each instance needs to be isolated from each other
  - *Partition*:
    - spreads instances across many different partitions (which rely on diff sets of racks) within an AZ.
    - scales to 100s of instances per group
    - up to 7 *partitions* per AZ
    - can span across multiple AZs in the same region
    - instances don't share racks with instances in other partitions
    - partition failure will affect one partition but leave rest ok
    - instances get access to the partition information as metadata
    - use case:
      - app that is partition aware to distribute data across partitions
      - Kafka / Cassandra / HBase

## ENI (elastic network interface)

- logical component in a VPC that represents a *virtual network card*
- can have:
  - *primary* private IPv4 && one or more secondary IPv4
  - *one* EIP (IPv4) *per* private IPv4
  - *one* public IPv4
  - one or more security groups
  - a MAC address
- can create ENI independently and attach them on the fly on EC2 instances for failover
- bound to AZ

## EC2 Hibernate

- no stop/start
- in-memory state (RAM) is preserved
- faster boot time
- RAM state is written to a file in the *root* EBS volume
- root EBS *volume must be encrypted*
- EBS must be big enough to save RAM dump
- instance ram size must be less that 150gb
- not supported for bare meatal
- available for on-demand, spot and reserved
- cannot be hibernated for more than 60 days
- use case:
  - long running processes
  - saving the RAM state
  - services that take time to initialize

### Topics that might come up

#### EC2 Nitro

- underlying platform for the next gen of EC2 instances
- new virtualization technology
- allows better performance
  - enhanced network: HPC, IPv6
  - higher speed EBS (nitro is necessary for 64K EBS IOPS - max 32K on non-nitro)
- better underlying security
- instance type examples
  - virtualized: A1, C5, C6, D3, G4, M5, etc.
  - bare metal: a1.metal, c5.metal, c6g.metal, etc.

#### Understanding vCPU

- only specified at launch
- multiple threads can run on a CPU (multithreading)
- each thread is represented as a virtual CPU
- eg: m5.2xlarge
  - 4 CPU
  - 2 threads per CPU
  - 8 vCPU in total
- use case:
  - optimizing CPU options
    - number of CPU cores
      - instances come with a combination of vCPU and RAM
      - may want to reduce number of vCPU => helpful when you need high RAM and less CPU (eg: reduce license cost per CPU)
    - number of threads per core
      - disable multithreading to have 1 thread per CPU
      - good for HPC (high performance computing) workloads

#### Capacity Reservations

- capacity reservation ensures you have EC2 capacity when needed
- manual or planned end of dater reservation
- no need for 1/3 year commitment
- access is immediate - billed as soon as it starts
- specify:
  - AZ in which to reserve capacity (if > 1 AZ needed => need to specify again)
  - number of instances for which to reserve capacity
  - instance tenancy - OS - instance type
- combine with `savings plan` and `reserved instances` for cost saving
