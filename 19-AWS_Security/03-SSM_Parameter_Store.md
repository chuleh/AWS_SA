# SSM Parameter Store

* reference parameters from command or code
* secure storage for configuration and secrets
* optional seamless encryption using KMS
* serverless, scalable, durable, easy SDK
* version tracking of config/secrets
* security thru IAM
* notifications with EventBridge
* integration with CloudFormation

# SSM Hierarchy

* /my-department/
  * my-app/
    * dev/
      * db-url
      * db-password
    * prod/
      * db-url
      * db-password
  * other-app/
* /other-department/

* by id
  * /aws/reference/secretsmanager/secret_ID_in_Secrets_Manager
* public ones
  * /aws/service/ami-amazon-linux-latest/amzn2-ami-hvm-x86_64-gp2

## Standard and advanced parameters

* standard
  * total number of parameters allowd (per account and region): 10K
  * max size of parameter value: 4kb
  * parameter policies available: no
  * cost: no additional charge
  * storage pricing: free
* advanced
  * total number of parameters allowd (per account and region): 10oK
  * max size of parameter value: 8kb
  * parameter policies available: yes
  * cost: charges apply
  * storage pricing: $0,05 per advanced parameter per month

## Paremeter Policies (for advanced only)

* allows to assign a TTL to a parameter (expiration date)
  * force updating or deleting sensitive data
* can assign multiple policies at a time
* expiration notification thru EventBridge
