# EKS - Amazon Elastic k8s service

* way to launch managed k8s clusters on AWS
* similar goal to ECS but diff API
* EKS supports EC2 if you want to deploy *worker nodes* or Fargate to deploy *serverless containers*
* use case: company already using k8s (on-prem or diff cloud) and want to migrate to AWS

* Node Types:
  * Managed Node Groups:
    * creates and manages nodes (EC2 instances) for you
    * nodes are part of an ASG managed by EKS
    * supports On-Demand or Spot instances
  * Self-Managed Nodes:
    * nodes created yourself and registered to the EKS cluster and managed by and ASG
    * can use prebuilt AMI - Amazon EKS optimized AMI
    * supports On-Demand or Spot instances
  * Fargate:
    * no maintenance required
    * no nodes managed

* EKS Data Volumes
  * need to specify *StorageClass* manifest on your EKS cluster
  * leverages a *Container Storage Interface* (CSI) compliant driver
  * support for:
    * EBS
    * EFS (only type of storage class that works with Fargate)
    * FSx for Lustre
    * FSx for NetApp ONTAP
