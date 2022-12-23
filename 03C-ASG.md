# What is an Auto Scaling Group?

* the load on your websites and apps can change => in the cloud you can create and get rid of servers fast
* the goal of an ASG is to:
  * scale out (add EC2 instances) to match an increased load
  * scale in (remove EC2 instances) to match a reduced load
  * ensure we have a minimum and a maximum number of EC2 instances running
  * automatically register new instances to an LB
  * re-create an EC2 instance in case previous one is terminated (ex: unhealthy)
  * ASG is free -you only pay for the underlying EC2 instances-
  * you set a minimum capacity / set desired capacity / set maximum capacity
  * ELB can check health of your instances on ASG
  * ELB will send traffic when you scale out

* ASG Attributes
  * AMI + Instance Type
  * EC2 User Data
  * EBS Volumes
  * Security Groups
  * SSH Key Pair
  * IAM Roles for your EC2 instances
  * Network + Subnets Information
  * Load Balancer Information
  * Min Size / Max Size / Initial Capacity
  * Scaling policies
    * Integration with CloudWatch
    * ex: An alarm monitors a metric (Such as Avg CPU  or a *custom metric*)

* ASG - Dynamic Scaling Policies
  * Target Tracking Scaling
    * most simple and easy to set up
    * ex: avg cpu stay around 40%
  * Simple / Step Scaling
    * When a CloudWatch alarm is triggered (ex cpu > 70%) then add 2 units
    * When a CloudWatch alarm is triggered (ex cpu < 30%) then remove 1
  * Scheduled Actions
    * Anticipate a scaling based on known usage patterns
    * ex: increate the min capacity to 10 at 5 pm fridays
  * Predictive Scaling
    * continuosly forecast load and schedule scaling ahead

* Good metrics to scale on
  * *CPUUtilization* avg cpu utilization across instances
  * *RequestCountPerTarget* to make sure the number of requests per EC2 instance is stable
  * *AVG Network In / Out* if your application is network bound
  * *Any custom metric* that you push using CloudWatch

* Scaling Cooldowns
  * after a scaling activity happens, you are in the *cooldown period* (default 300 seconds)
  * during the cooldown period, the ASG will not launch or terminate additional instances (to allow for metrics to stabilize)
  * *ADVICE* use a ready-to-use AMI to reduce configuration time in order to be serving requests faster and reduce cooldown


## ASG for Solutions Architect
* ASG default termination policy
  * Find the AZ which has the most number of instances
  * If there are multiple instances in the AZ, delete the one with the oldest launch configuration
  * ASG tries to balance the numbers of instances across AZ.
* Scaling cooldowns
  * helps to ensure your ASG doesnt launch or terminate additional instances before the previous scaling activity takes effect
  * We can create cooldowns that apply to a specific simple scaling policy
  * A scaling-specific cooldown period overrides the default cooldown period
  * Default cooldown period of 300 secs can be too long > reduce costs by applying a scaling specific cooldown period of 180 secs to the scale in policy
  * If your application is scaling up and down multiple times each hour, modify the ASGs cool-down timers and the cloudwatch alarm period that triggers the scale in
