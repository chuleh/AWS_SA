# CloudWatch Logs for EC2

* by default no logs from EC2 will go to CloudWatch
* need to run a CloudWatch agent on EC2 to push logs to CloudWatch
* make sure IAM permissions are correct - needs role
* CloudWatch log agent can be set up on prem

## CloudWatch Logs Agent && Unified Agent

* for virtual servers or on-prem

* CloudWatch Logs Agent
  * old version of the agent
  * can only send logs to cloudwatch

* CloudWatch Unified Agent
  * collect additional system-level metrics such as RAM, processes, etc
  * collects logs to send to CloudWatch logs
  * centralized configuration using SSM Parameter Store

## CloudWatch Unified Agent - Metrics

* collected directly on your Linux server / EC2 Instance
* CPU (active, guest, idle, systme, user, steal)
* disk metrics (free, used, total) / Disk IO (writes, reads, bytes iops)
* RAM (free, inactive, used, total, cached)
* netstat (number of TCP and UDP connections, net packets, bytes)
* processes (total, dead, bloqued, idle, running, sleep)
* swap space (free, used, used %)
