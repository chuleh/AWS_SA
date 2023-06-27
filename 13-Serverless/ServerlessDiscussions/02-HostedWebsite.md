# Serverless Hosted Website

* want to create
  * should scale globally
  * rarely written / often read
  * some of the website is purely static files / rest is a dynamic REST API
  * caching implemented where possible
  * new subs should receive email
  * photos uploaded to blog should have thumbnail generated (serveless)

* solution:
    * content in S3
    * expose bucket globally thru CloudFront => client interacts with edge locations
  * serving content securely
    * add OAC (Origin Access Control)
    * ensures bucket can only be accessed by CloudFront via IAM policy
  * dynamic rest API + caching => no need of cognito cause it's public
    * REST HTTPS <==> API GW <==> invoke Lambda <==> query/read DAX <==> DynamoDB
    * optional: *global tables*
  * email for user subbed
    * DynamoDB stream <==> invoke lambda
    * lambda <==> SDK to send email via *SES*
  * thumbnail generation
    * client uploads image onto S3
      * could also be thru an OAC
      * use Transfer Acceleration
    * S3 triggers Lambda
     * creates thumbnail
     * can upload to a diff bucket

