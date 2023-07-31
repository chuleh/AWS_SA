# Advanced Policies

## IAM conditions

* restric/allow access to resources based on conditions
* can be used to enforce MFA && stop/start/terminate instances only if MFA validated
* other conditions
  * Can be:
    * regions
    * ips
    * etc
    * ec2Tags (based on tags)
* step policies => ex S3
  * S3:ListBucket permission applies to an *ARN* => bucket level permission
    * arn:aws:s3:::test
  * S3:GetObject, s3:PutObject, s3:DeleteObject applies to a diff *ARN*
    * arn:aws:s3:::test/*
    * object level permission

## Resource Policies && aws:PrincipalOrgID

* *aws:PrincipalOrgID* can be used in any resource policies to restric access to accounts member of an org
 * aws:PrincipalOrgID: [x-y]
   * access to bucket on S3
   * user outside organization does not
