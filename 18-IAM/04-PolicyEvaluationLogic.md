# IAM Policy Logic

## IAM Permission boundaries

* IAM permission boundaries are supported for users and roles (not groups)
* advanced feature to use a managed policy to set the maximum permissions an IAM entity can get
* ex: user has a *permission boundary* => allow s3:*, cloudwatch:*, ec2:*
  * and *IAM permissions* through *IAM policy* => allow iam:CreateUser
  * no IAM permissions => iam permission is outside the boundary
* permission boundary overrides user permission
  * if user has AdminAccess *but* permission boundary is an S3 attached policy he only has access to S3
* can be used in combinations of AWS Organizations SCP
* use cases:
  * delegate responsibilites to non-admins within their permission boundaries => create IAM users
  * allow devs to self-assign policies => manage their own permissions and make sure can't escalate privileges
  * useful to restrict one specific user (instead of a whole Org && SCP)
