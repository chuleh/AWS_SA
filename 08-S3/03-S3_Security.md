# Encryption

* 4 methods for encrypting in S3
  * SSE-S3: encrypts objects using keys handled and managed by AWS
    * object is encrypted server side
    * AES-256 encryption
    * must set header "x-amz-servers-side-encryption":"AES256"
    * enabled by default for new buckets and new objects
  * SSE-KMS: leverage AWS KMS (Key management service) to manage encryption keys
    * advantages: user control + audit trail
    * must set header "x-amz-servers-side-encryption":"aws:kms"
    * AWS manages keys
    * you have control of the key rotation policy
    * Limitations:
      * *may be* impacted by the KMS limits
        * when you upload, it calls the *GenerateDataKey* KMS API
        * when  you download, it calls the *Decrypt* KMS API
        * count towards the KMS quota per second (5500 / 10000 / 30000 req/s based on region)
        * can request a quota increase using the Serivce Quotas Console
  * SSE-C: when you want to manage your own encryption keys
    * SSE using data keys fully managed by the customer outside of AWS
    * AWS S3 does not store the encryption key you provided
    * HTTPS must be used
    * Encryption key must be provided in HTTP headers, for every http request made
  * Client Side Encryption
    * Client library such as the AWS S3 Encryption Client
    * Client must encrypt data themselves before sending to S3
    * Client must decrypt data themselves when retrieving from S3
    * Customer fully manages the keys and the encryption cycle

* Encryption in transit (SSL)
  * HTTP endpoint: non encrypted
  * HTTPS endpoint : encryption in flight

* default encryption
  * SSE-S3 encryption is automatically applied to new objects stored in S3 buckets
  * optionally, you can "force encryption" using a bucket policy and refuse any API call to *PUT* an S3 object without
    encryption headers (SSE-KMS or SSE-C)
* bucket policies are evaluated *before* default encryption

## S3 Cors

* Cross-Origin Resource Sharing
* web browser based mechanism to allow requests to other origins while visiting the main origin
* Origin = scheme (protocol) + host (domain) + port
  * example: <https://www.example.com> (port == 80 || 443)
* If you request data from another S3 bucket, you need to enable CORS
* Cross Origin Resource Sharing allows you to limit the number of websites that can request your files in S3 (and limit
  your costs)

## S3 MFA Delete

* MFA forces user to generate a code on a device befoire doing important operations on S3
* To use MFA-Delete, enable Versioning on the S3 Bucket
* You will need MFA to:
  * permanently delete an object version
  * suspend versioning on the bucket
* You will not need MFA for:
  * enabling versioning
  * listing deleted versions
* Only the bucket owner (root account) can enable/disable MFA-Delete
* MFA Delete currently can only be enabled using the CLI

## S3 Access Logs

* For audit purposes you may want to log all access to S3 buckets
* Any request made to S3, from any account, authorized or denied, will be logged into another S3 bucket
* data can be analyzed with Athena
* target logging bucket must be in the same AWS region
* *do not* set your logging bucket to be a monitored bucket => it will create a logging loop and your bucket will grow exponentially

## S3 Pre-signed URLs

* Can generate pre-signed urls using SDK or CLI
  * For downloads, easy, can use the CLI
  * For uploads, harder, must use the SDK
* Valid for a default of 3600 seconds, can change timeout with --expires-in- [TIME_BY_SECONDS] argument
* Users given a pre-signed URL inherit permissions of the person who generated the URL for GET/PUT
  * Ex: Allow only logged-in users to download a premium video on your S3 bucket
  * Ex: Allow an ever changing list of users to download files by generating URLs dynamically
  * Ex: Allow temporarily a user to upload a file to a precise location in our bucket

## S3 Object Lock & Glacier Vault Lock

* S3 Object Lock
  * versioning must be enabled
  * Adopt a WORM (Write Once Read Many) model
  * block an object version deletion for a specified amount of time
  * retention mode - compliance
    * object versions can't be overwritten or deleted by any user (even root)
    * objects retention modes can't be changed and retention periods can't be shortened
  * retention mode - governance
    * most users can't overwrite or delete an object version or alter its lock settings
    * some users have special permissions to change the retention or delete the object
    * in *both modes* retention period must be specified / it can be extended
    * legal hold: protects object indefinitely, independently from retention period
      * can be removed using the *s3:PutObjectLegalHold* IAM Permission
* Glacier Vault Lock
  * Adopt a WORM (Write Once Read Many) model
  * Lock the policy for future edits (can no longer be changed)
  * Helpful for compliance and data retention

## S3 - Access Points

* AP for groups to access different buckets
* each AP gets its own DNS and policy to help limit who can access it
  * a specific IAM user / group
  * one policy per Access Point => easier to manage than complex bucket policies

## S3 Object Lambda

* use AWS Lambda Functions to change the object before it is retrieved by the caller app
* Only one S3 bucket is needed, on top of which we create *S3 Access Point* and *S3 Object Lambda Access Point*

## S3 Consistency model

* Read after write consistency for PUTS of new objects
  * as soon as an object is written, we can retrieve it (PUT 200 -> GET 200)
  * this is true *EXCEPT* if we did a GET before to see if the object existed (GET 404 -> PUT 200 -> GET 404) -
    *eventually consistent*
* Eventual consistency for DELETES and PUTS of existing objects
  * if we read an object after updating, we might get the older version
    (PUT 200 -> PUT 200 -> GET 200 (might be older version))
  * if we delete an object, might still be able to retrieve it for a short time
    (DELETE 200 -> GET 200)
