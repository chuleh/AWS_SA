# ECS - Elastic Container Service

* Launch docker containers on AWS == launch *ECS tasks* on *ECS clusters*
* EC2 launch type:
  * must provision and maintain the infrastructure (EC2 instances)
  * must specify ASG
  * each EC2 instance must run the *ECS agent* to register in the *ECS cluster*
  * AWS takes care of starting/stopping containers
* Fargate launch type:
  * launch docker containers on AWS
  * no infra provisioned (no EC2 instances) == serverless
  * just create task defitions
  * AWS just runs ECS tasks definition for you based on the CPU/RAM you need
  * to scale just increase the number of tasks

## IAM roles for ECS

* EC2 instance profile (EC2 Launch type *only*)
  * used by the ECS agent
  * make API calls to ECS service
  * send container logs to CloudWatch Logs
  * pull docker image from ECR
  * reference sensitive data in SecretsManager or SSM Parameter store
* ECS task role
  * valid for both launc types
  * allow each task to have a specific role
  * use diff roles for the different ECS services you run
  * task role defined in *task definition*

## Load Balancer integrations

* ALB supported and works for most cases
* NLB recommended only for hi thruput or paired with AWS Private Link
* CLB supported but not recommended (no advanced features - no Fargate)

## Data Volumes (EFS)

* mount EFS file systems onto ECS tasks
* works for both EC2 and Fargate *launch types*
* tasks running in any AZ will share the same data in the EFS file system
* Fargate + EFS == serverless

* Use case: persistent multi-AZ shared storage for your containers
  * S3 cannot be mounted as file system

## ECS AutoScaling

* automatically increase/decrease the desired number of ECS tasks
* ECS AutoScaling uses AWS Application AutoScaling
  * ECS service average CPU utilization
  * ECS service average Memory utilization (RAM)
  * ALB request count per target - metric coming from ALB
* Traget Tracking - scale based on target value for a specific CloudWatch metric
* Step Scaling - scale based on a specified CloudWatch alarm
* Scheduled Scaling - scale based on a specified date/time (predictable changes)

* NOTE: ECS Service Auto Scaling == task level != EC2 Auto Scaling (EC2 instance level)
* Fargate AutoScaling much easier to set up due to serverless

## EC2 Launch Type - AutoScaling EC2 instances

* accomodate EC2 service scaling by adding underlying EC2 instances
* AutoScaling Group Scaling
  * scale your ASG based on CPU utilization
  * add EC2 instances over time
* ECS Cluster Capacity Provider
  * used to automatically provision and scale the infrastructure for your ECS tasks
  * Capacity Provider paired with an AutoScaling Group
  * add EC2 instances when youe missing capacity (CPU, RAM)
