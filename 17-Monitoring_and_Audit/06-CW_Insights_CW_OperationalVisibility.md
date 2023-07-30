# CloudWatch Insights and Operational Visibility

## CW Container Insights

* collects, aggregate, summarize metrics and logs from containers
* available for containers on:
  * ECS
  * EKS
  * k8s platform on EC2
  * Fargate (both ECS and EKS)
* in Amazon EKS and K8s, CW Insights is using a containerized version fo the CW Agent to discover containers

## CW Lambda Insights

* monitoring and troubleshooting solutions for serverless apps running on Lambda
* collectes, aggregates and summarizes system-level metrics including CPU time, memory, disk and network
* collectes, aggregates and summarizes diagnostic information such as cold starts and Lambda workers shutdown
* Lambda insights is provided as a Lambda layer

## CW Contributor Insights

* analyze log data and create time series that display contributor data
  * see metrics about the top-N consumers
  * the total number of unique contributors and their usage
* helps you find top talkers and understand who or what is impacting system performance
* works for any AWS-generated logs (VPC, DNS, etc)
  * ex: find bad hosts, identify heaviest network users or find the URLs that generate most errors
* can build rules from scratch or you can also use sample rules created by AWS-generated
  * leverages your *CloudWatch Logs*
* also provides built-in rules that you can use to analyze metrics from other AWS services

## CW Application Insights

* provides automated dashboards that show potential problems with monitored apps to help isolate ongoing issues
* apps run on EC2 instances with selected techs only => Java, .NET, IIS Web Server, databaes, etc.
  * you can use other AWS resources such as: EBS, RDS, ELB, Lambda, DynamoDB, etc
* powered by SageMaker
  * enhanced Visibility into your application health to reduce the time it will take you troubleshoot and repair
* findings and alets are sent to EventBridge and SSM OpsCenter
