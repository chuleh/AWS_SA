# Mobile App: mytodo list

* want to create a mobile app with the following requirements
  * expose as REST API with HTTPS
  * serverless
  * user should be able to directly interact with their own folder in S3
  * user should be able to authenticate through a managed serverless service
  * users can read and write to-dos but mostly read them
  * db should scale and have some high read throughput

* solution
  * mobile client => REST HTTPS API GW <==> invoke Lambda <==> query DynamoDB
    * authenticate <==> Cognito
    * API GW <==> Cognit - verifies authentication
  * giving clients access to S3
    * authenticates to Cognito
    * gives temp creds <==> aws sts
    * returns creds to mobile client
    * creds allow to store retrieve files from bucket (S3)
    ⚠️  wrong to store credentials in client
  * high read throughput
    * 2 ways
      * DAX in front of DynamoDB (cache) == caching the reads
      * cache responses at AP GW (if answers don't change much) == caching the REST requests at api level

