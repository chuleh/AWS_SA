# Disaster Recovery in AWS

* disaster == any event that
  * has a negative impact on a company's business continuity
  * negative impact on finance
* disaster recovery ==  preparing for and recovering from a disaster
* hybrid recovery == on prem => aws
* cloud recovery == aws region A => region B

## RPO && RTO

RPO: recovery point objective
RTO: recovery time objective

RPO:

* how often you back up data
* how much data is lost between disaster and an RPO

RTO:

* when you recover from the disaster
* time between disaster and RTO == downtime

## Disaster Recovery Strategies

* strats on prem to aws
  * backup and restore
    * HIGH RPO && RTO
      * from DataCenter to AWS (ex) => Storage GW / SnowBall => S3 => Glacier
      * on AWS => EBS / RedShift / RDS => scheduled regular snapshot => storage
      * cheap
  * pilot light
    * small version of app always running in the cloud
    * useful for critical core (pilot light)
    * very similar to backup and restore
    * faster => critical systems already up
    * ex: app on DC => sends data to DB && replicated on RDS
      * RDS running && EC2 *not* running
      * DC crashed => start EC2
      * Route53 => EC2 instances
      * lower RTO && RPO
  * warm standby
    * full system up and running and minimum size
    * ex: DC with reverse proxy => app server => DB
      * data replication on DB => RDS (running as slave)
      * EC2 + ASG running at min
      * failover == Route53 => LB => EC2 => read from Slave on RDS
      * more cost
      * less RPO && RTO
  * hot site / multi approach
    * very low RTO - very expensive
    * full prod scale running on AWS && on-prem
    * on failover Route53 => EC2
  ==> faster RTO == more cost
* all AWS
  * multi-region
    * replace RDS with Aurora Global (Master => Slave)

## DR Tips

* backup
  * EBS snapshots
  * RDS automated backups
  * regular pushes to S3 / S3 IA / Glacier Lifecycle policy / Cross-region replication
  * on-prem to aws => storage gateway / snowball
* high availability
  * route53 migrate dns from region to region
  * multi-az
    * rds
    * elasticache
    * EFS
    * S3
  * S2S VPN to recover from DirectConnect
* replication
  * RDS replication (cross region)
  * AWS Aurora + Global Databases
  * DB replication from on-prem to RDS
  * Storage Gateway
* automation
  * Cloudformation / Elastic Beanstalk to recreate a whole new environment
  * recover / reboot EC2 instances with CloudWatch alarms on fail
  * Lambda for customized automations
* chaos
  * netflix like practice (simian army)
