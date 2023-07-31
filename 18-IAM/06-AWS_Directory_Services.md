# AWS Directory Services

* Directory Services for ActiveDirectory
  * found on any Windows Server with AD Domain Services
  * Database of objects: User Accounts, Computers, Printers, File Shares, Security Groups
  * centralized security mgmt, create account, assign permissions
  * objects organized in trees
  * group of trees is a forest
* 3 flavours
  * AWS Managed Microsoft AD
    * create your own AD in AWS, manage users locally, supports MFA
    * establish "trust" connections with your on-prem AD
  * AD Connector
    * Directory Gateway (proxy) to redirect to on-premise AD, supports MFA
    * users ara managed on the on-prem ActiveDirectory
  * Simple AD
    * AD-compatible managed directory on AWS
    * cannot be joined with on-prem AD

## Integrate IAM Identity Center with ActiveDirectory

* connect to an AWS Managed Microsoft AD (Directory Service)
  * out of the box
* connect to a self-managed directory
  * create a two-way Trust Relationship using AWS Managed Microsoft AD
    * Identity Center => connect to AWS Managed Microsoft AD <=> two way trust relationship => ActiveDirectory
    * better for managing users in the cloud
  * create an AD Connector
    * Identity Center => connect to AWS AD Connector <=> proxy => ActiveDirectory
    * better for on-prem > latency
