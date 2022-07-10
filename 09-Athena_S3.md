# S3 MFA Delete
* MFA forces user to generate a code on a device befoire doing important operations on S3
* To use MFA-Delete, enable Versioning on the S3 Bucket
* You will need MFA to:
  - permanently delete an object version
  - suspend versioning on the bucket
* You will not need MFA for:
  - enabling versioning
  - listing deleted versions
* Only the bucket owner (root account) can enable/disable MFA-Delete
* MFA Delete currently can only be enabled using the CLI

## S3 Access Logs
* For audit purposes you may want to log all access to S3 buckets
* Any request made to S3, from any account, authorized or denied, will be logged into another S3 bucket

## S3 Cross Region Replication
* Must enable versioning in source and destination
* Must be in different aws regions
* Can be in different accounts
* Copying is asynchronous
* Must give proper IAM permissions to S3
* Use cases: compliance, lower latency access, replication accross accounts

## S3 Pre-signed URLs
* Can generate pre-signed urls using SDK or CLI
  - For downloads, easy, can use the CLI
  - For uploads, harder, must use the SDK
* Valid for a default of 3600 seconds, can change timeout with --expires-in- [TIME_BY_SECONDS] argument
* Users given a pre-signed URL inherit permissions of the person who generated the URL for GET/PUT
  - Ex: Allow only logged-in users to download a premium video on your S3 bucket
  - Ex: Allow an ever changing list of users to download files by generating URLs dynamically
  - Ex: Allow temporarily a user to upload a file to a precise location in our bucket

## S3 Storage Classes
* S3 standard - general purpose
  - High Durability of objects across multiple AZ (11x9 durability)
  - 99.99 Availability over a given year
  - Can sustain 2 concurrent facility failures
  - Use Cases: Big Data Analytics - mobile & gaming applications, content distribution
* S3 Standard-Infrequent Access (IA)
  - Suitable for data that is less frequently accessed but requires rapid access when needed
  - High durability (11x9) of objects across multiple AZs
  - 99.9 availability
  - Low cost compared to S3 Standard
  - Can sustain 2 concurrent facility failures
  - Use cases: data store for disaster recovery, backups
* S3 One Zone-Infrequent Access
  - Same as IA but data stored in a single AZ
  - High Durability (11x9) of objects in a single AZ; data lost when AZ is destroyed
  - 99.5 Availability
  - Low latency and high throughput performance
  - Supports SSL for data at transit and encryption at rest
  - Low cost compared to IA (by 20%)
  - Use cases: storing secondary backup of copies of on-prem data, storing data you can recreate
* S3 Intelligent Tiering
  - Same low latency and high throughput performance of S3 standard
  - Small monthly monitoring and auto-tiering fee
  - Automatically moves objects between two access tiers based on changing access patterns
  - High Durability of objects across multiple AZ (11x9 durability)
  - Resilient against events that impact an entire AZ
  - Designed for 99.9 availability over a given year
* Amazon Glacier
  - Low cost object storage meant for archiving / backup
  - Data is retained for the longer team (10s of years)
  - Alternative to on-prem magnetic tape storage
  - 11x9 durability
  - Cost per storage per month ($0.004 / gb) + retrieval cost
    - 3 retrieval options
      - Expedited (1 to 5 minutes)
      - Standard (3 to 5 hours)
      - Bulk (5 to 12 hours)
  - Each item in Glacier is called _Archive_ (up to 40tb)
  - Archives are stored in _Vaults_
  - Minimum storage duration of 90 days
* Amazon Glacier Deep Archive
  - Long term storage
  - Cheaper
  - Retrieval
    - Standard (12 hours)
    - Bulk (48h hours)
    - Minimum storage duration of 180 days
* S3 Reduced Redundancy Storage (deprecated - omitted)

## S3 Lifecycle Policies
* You can transition objects between storage classes
* For infrequently accessed object, move them to STANDARD_IA
* For archive objects you don't need in real time, GLACIER or DEEP_ARCHIVE
* Moving objects can be automated using a lifecycle configuration
  - Transition actions
    - It defines when objects are transitioned to another storage class
    - Move objects to STANDARD_IA class 60 days after creation
    - Move to Glacier for archiving after 6 months
  - Expiration Actions
    - Configure objects to expire (Delete) after some time
    - Access Log files can be set to delete after 365 days
    - Can be used to delete old versions of files (if versioning is enabled)
    - Can be used to delete incomplete multi-part uploads
* Rules can be created for a certain prefix (ex - S3://mybucket/mp3/asterisco)
* Rules can be created for certain objects tags (ex - Department: Finance)

## S3 Baseline Performance
* S3 automatically scales to high request rates - latency 100-200ms
* Your app can achieve at least 3.5k PUT/COPY/POST/DELETe and 5.5k GET/HEAD requests per second per prefix in a bucket
* Therre are no limits to the number of prefixes in a bucket
* If you spread READS across all four prefixes evenly, you can achieve 22k request per second for GET and HEAD

### Performance
* Multi-part Upload
  - recommended for files > 100MB
  - must use for files > 5GB
  - can help parallelize uploads
* S3 Transfer acceleration (upload only)
  - increase transfer speed by transferring file to an AWS edge location which will forward the data to the S3 bucket in
    the target region
  - compatible with mult-part upload
* S3 Byte-range fetches
  - Parallelize GETs by requesting specific byte ranges
  - Better resilience in case of failures
  - can be used to speed up downloads
  - can be used to retrieve only partial data (for example head of the file)

## KMS Limitation
* If you use SSE-KMS, you may be impacted by the KMS limits
  - when you upload, it calls the _GenerateDataKey_ KMS API
  - when you download, it calls the _Decrypt_ KMS API
  - count towards the KMS quota per second (5500, 10000, 30000 reqs based on region)
  - as of today, you cannot request a quota increase for KMS
  - if more reqs than region can handle > throttle


## S3 Select & Glacier Select
* Retrieve less data using SQL by performing _server side filtering_
* Can filter by rows & columns (simple sql statements)
* Less network transfer, less CPU cost client-side

## S3 Object Lock & Glacier Vault Lock
* S3 Object Lock
  - Adopt a WORM (Write Once Read Many) model
  - block an object version deletion for a specified amont of time
* Glacier Vault Lock
  - Adopt a WORM (Write Once Read Many) model
  - Lock the policy for future edits (can no longer be changed)
  - Helpful for compliance and data retention

## Athena
* Serverless service to perform analytics directly against S3 files
* Use SQL language to query the files
* Has a JDBC/ODBC driver
* Charged per query and amount of data scanned
* Supports CSV, Json, ORC, Avro and Parquet (built on Presto)
* Use cases:
  - business intelligence / analytics / reporting, analyze & query / vpc flow logs / ELB logs / CloudTrail tails / etc
  - Exam Tip: Analyze data directly on S3 => use Athena
