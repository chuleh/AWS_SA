# Advanced S3

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

## S3 Analytics

*  helps you decide when to transition objects to the right storage class
* recommendations for *standard* and *standard IA*
  * *does not* work for One-Zone IA and Glacier
* report is updated daily
* can take from 24 to 48 hours to see data analysis

## S3 Requester pays

* In general bucket owners pays storage and data transfer associated with bucket
  * with *requester pays buckets* the requester instead of the bucket owner pays the cost of the request and the data
    download from the bucket
  * requester *must be* authenticated in AWS (no anonymous)

## S3 Event notifications

* Automatically react to certain events on S3
  * use case: generate thumbnails of images uploaded to s3
  * object name filtering possible (\*.jpg)
  * can create as many events as desired
  * notifications can take from seconds to minutes
* S3 events notifications with EventBridge
  * event => bucket => EventBridge => over 18 aws services as destinations
  * advanded filtering with JSON
  * multiple destinations 
  * EventBridge capabilites - archive, replay events, reliable delivery

## S3 Baseline Perfirmance

* S3 automatically scales to high request rates - latency 100-200ms
* Your app can achieve at least 3.5k PUT/COPY/POST/DELETE and 5.5k GET/HEAD requests per second per prefix in a bucket
* There are no limits to the number of prefixes in a bucket
* If you spread READS across all four prefixes evenly, you can achieve 22k request per second for GET and HEAD

## S3 Performance

* Multi-part Upload
  * recommended for files > 100MB
  * must use for files > 5GB
  * can help parallelize uploads
* S3 Transfer acceleration (upload only)
  * increase transfer speed by transferring file to an AWS edge location which will forward the data to the S3 bucket in the target region
  * compatible with multi-part upload
* S3 Byte-range fetches
  * speed up downloads
  * Parallelize GETs by requesting specific byte ranges
  * Better resilience in case of failures
  * can be used to speed up downloads
  * can be used to retrieve only partial data (for example head of the file)

## S3 Select & Glacier Select

  * retrieve less data using SQL by performing server side filtering
  * can filter by rows & columns (simple SQL statements)
  * less network transfer, less CPU cost client-side

## S3 batch operations

* perform bulk operations on existing S3 objects with a single request
  * modify object metadata & properties
  * copy objects between S3 buckets
  * encrypt un-encrypted objects
  * modify ACLs, tags
  * restore objects from S3 Glacier
  * Invoke Lambda function to perform custom action on each object
* a job consists of a list of objects, the action to perform and a list of parameters
* S3 batch operations manages retries / tracks progress / sends completion notifications / generates reports / etc
* you can use *S3 Inventory* to get object list and use *S3 Select* to filter your objects
