# AWS KMS - Key Management Service

* anytime you hear "encryption" for an AWS service it's most likely KMS
* AWS manages encryption keys for us
* fully integrated with IAM for authorization
* easy way to control access to your data
* able to audit KMS key usage with CloudTrail
* integrated with most AWS services (EBS, S3, SSM, RDS)
* *never* store secrets in plaintext
  * KMS Key encryption also available thru API calls (SDK, CLI)
  * encrypted secrets can be stored in code/environment variables

## KMS Key Types

* KMS Keys is the new name of KMS Customer Master Key
* Symmetric (AWS-256 keys)
  * one single encryption key that is used to encrypt and decrypt
  * AWS services that are integrated with KMS Symmetric CMKs
  * you *never* get access to the KMS Key Unencrypted (must call KMS API to use)
* Assymetric (RSA && ECC Key Pairs)
  * public (encrypt) and private key (decrypt) pair
  * used for encrypt/decrypt or sign/verify operations
  * pub key is downloadable *but* you can't access the private key unencrypted
  * use: encryption outside AWS by user who can't call the KMS API
* types of managed keys
  * AWS Owned keys (free): SSE-S3, SSE-SQS, SSE-DDB (default key)
  * AWS Managed keys (free): (aws/service-name | ex: aws/rds)
  * Customer Managed Keys created in KMS: $1/mo
  * Customer Managed Keys imported (must be Symmetric): $1/mo
  * pay for API call to KMS ($0,03 / 1000 calls)

## Automatic Key Rotation

* AWS-managed KMS Key: automatic every 1 year
* Customer-managed KMS Key: (must be enabled) automatic every 1 year
* Imported KMS Key: only manual rotation possible using alias

## Copying Snapshots across regions

* eu-west-2
  * EBS volume encrypted with KMS Key A => snapshot encrypted with KMS Key A
  * move to ap-southeast-2
    * reencrypt with KMS Key B
    * decrypt with KMS Key B

## KMS Key Policies

* control access to KMS Keys -similar to S3 policies-
  * diff: cannot control access without them => if no KMS Policy *no one* can access them
* Default KMS Key Policy
  * created if you don't provide a specific KMS Key Policy
  * complete access to the key to the root user == entire AWS Account
* Custom KMS Key Policy:
  * define users, roles, that can access the KMS key
  * define who can administer the key
  * useful for cross-account access of your KMS key

## Copying Snapshots across accounts

* create snapshot, encrypted with own KMS Key
* attach a KMS Key Policy to authorize cross-account access
* share the encrypted snapshot
* (in target) create a copy of the snapshot => encrypt with CMK in account
* create volume from snapshot

## KMS Multi-Region Keys

* primary key can be replicated and syncd with several regions
* key id *will be the same* across all regions *NOT the ARN*
* use case:
  * identical KMS Keys in diff AWS Regions - can be used interchangeably
  * have the same key ID, key material, auto rotation, etc
  * encrypt in one region and decrypt in other region => no need to re-encrypt when moving data from region to region
  * no need to make cross-region API calls
* KMS Multi-Region are *NOT* global (Primary + Replica)
* each Multi-Region key is managed *independently*
* usually *not recommended* unless using global client-side encryption (Global DynamoDB, Global Aurora)

## DyamoDB Global Tables and KMS Multi-Region Keys client-side encryption

* we can encrypt sepcific attributes client-side in our DyamoDB table using the *Amazon DyamoDB Encryption Client*
  * table not needed since it's at rest
  * data is only available to specific client who has the key (not even available for DB Admins)
  * combined with global tables => client-side encrypted data is replicated to other regions
  * if using Multi-Region key, replicated in the same region as the DynamoDB Global Table
    * => clients in these regions can use low-latency API calls to KMS in their region to decrypt the data client-side

## Global Aurora and KMS Multi-Region Keys client-side encryption

* same concept
* can encrypt specific attributes client-side in our Aurora table using the *AWS Encryption SDK*
  * combined with aurora global tables => client-side encrypted data is replicated to other regions
  * if using Multi-Region key, replicated in the same region as the Aurora Global Table
    * => clients in these regions can use low-latency API calls to KMS in their region to decrypt the data client-side

## S3 Replication Encryption Considerations

* unencrypted objects and objects encrypted with SSE-S3 are replicated by default
* objects encrypted with SSE-C (customer provided key) are never replicated
  * would ask for key every single time
* objects encrypted with SSE-KMS you need to enable the option
  * specify which kms key to encrypt objects within the target bucket
  * adapt the KMS Keys Policy for the target key
  * an IAM role with *kms:Decrypt* for the source KMS Key and *kms:Encrypt* for the target key
  * you might get KMS throttling erros => if so ask for a *Service Quota Increase*
* Good to know: you can use Multi-Region AWS KMS keys, but they are *currently treated as independent keys by S3*
  * object *will still* be decrypted and reencrypted

## Encrypted AMI sharing process

* AMI in source account is encrypted with KMS Key from Source Account
* must modify the image attribute to add a *Launch Permission* which corresponds to the specified target AWS Account
* must share the KMS Keys used to encrypt the snapshot AMI references with the target account / IAM role
* the IAM role/user in the target account must have the permissions to *DescribeKey*, *ReEncrypted*, *CreateGrant*, *Decrypt*
* when launching EC2 instance from the AMI optionally the target account can specify a new KMS key
  * on its own account
  * used to reencrypt volumes
