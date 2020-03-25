# AWS CloudFront
* CDN (Content Delivery Network)
* Improves read performance - content is cached at the edge
* 216 points of presence globally (edge locations)
* DDoS protection
* Integration with Shield, AWS Web Application Firewall
* Can expose external HTTPS and can talk to internal HTTPS backends

## CloudFront Origins
* Origins - what cloudfront can source data from
  - S3 Bucket
    - For distributing files and caching them at the edge
    - Enhanced security with CloudFront Origin Access Identity (OAI)
    - CloudFront can be used as an ingress (to upload files to S3) gives you transfer acceleration
    ![alt text][logo]
    [logo]: ./CloudFrontS3Origin.png
  - S3 Website
    - Must first enable the bucket as a static S3 website
  - Custom Origin (HTTP) - Must be publicly accessible
    - Application Load Balancer
    - EC2 instance
    - Any HTTP backend you want
