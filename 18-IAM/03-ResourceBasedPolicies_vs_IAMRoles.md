# IAM Roles vs Resource Based Policies

* when you assume a role (user, application or service)
  * give up your original permissions and take the permissions assigned to the role
* when you use a resource-based policy the *principal does not* give up his permissions
* EventBridge scenario:
  * when a rule runs it needs permission on the target
  * resource-based policy: Lambda, SNS, SQS, CW Logs, etc
    * EventBridge Rule => Lambda with resource-based policy
  * IAM Role: Kinesis stream, Systems Manager Run Command, ECS Task
    * EventBridge Rule => IAM role for Kinesis

## cross account:
* attaching a *resouce-based policy* to a resource (ex: s3 bucket policy)
* OR using a *role* as a proxy
* ex:
  * user account A => assumes Role Account B => access to bucket
  * user account A => bucket policy => access to bucket

