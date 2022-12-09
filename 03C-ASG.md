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


