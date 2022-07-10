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
      - a user of CloudFront that will be accessing s3 bucket
      - creates a policy that allows cloudfront to acces S3 bucket
      - objects in bucket can be private and still will be accessed by CloudFront but not hitting directly the S3
        endpoint
    - CloudFront can be used as an ingress (to upload files to S3) gives you transfer acceleration
  - S3 Website
    - Must first enable the bucket as a static S3 website
  - Custom Origin (HTTP) - Must be publicly accessible
    - Application Load Balancer
      - must be public
      - ec2 instances can be private but SG must allow ALB
      - ALB SG must allow public IP of edge locations
    - EC2 instance
      - must be public
      - SG must allow ips from Edge Locations
    - Any HTTP backend you want

## CloudFront Geo Restriction
* You can restrict who can access your distribution
  - Whitelist: Allow your users to access your content only if they're in one of the countries on a list of approved
    countries
  - Blacklist: Prevent users from accessing your content if they're in one of the countries in one of the countries on
    the blacklist
  - The country is determined using a 3rd party Geo-IP Database
    - Use case: Copyright Laws to control access to content

## CloudFront vs S3 Cross Region Replication
* CloudFront:
  - Global Edge Network
  - Files are cached for a TTL (maybe a day)
  - Great for static content that must be available everywhere
* S3 Cross Region Replication
  - Must be setup for each region you want replication to happen
  - Files are updated in near real-time
  - Read only
  - Great for dynamic content that need to be avilable at low-latency in a few regions

## CloudFront Signed URL / Signed Cookies
* You want to distribute paid shared content to premium users over the world
* You want to see and know who has access to the CloudFront distribution
  - We can use a CloudFront Signed URL / Cookie => Attach a policy with
    - Includes URL expiration
    - Include IP ranges to access data from
    - Trusted signers (which AWS accounts can create signed URLs)
  - How long should the url be valid for?
    - Shared content (movie, music): make it short (a few mins)
    - Private content (private to the user): you can make it last for years
* Signed URL == access to individual files (one signed URL per file)
* Signed cookies == access to multiple files (one signed cookie for many files)

## CloudFront Signed URL vs S3 Pre-signed URL
* CloudFront signed url:
  - allow access to a path no matter the origin
  - account wide key-pair, only the root can manage it
  - can filter by IP, path, date, expiration
  - can leverage caching features
* S3 Pre signed URL:
  - Issue a request as the person pre-signed the url
  - Uses the IAM KEy of the signing IAM principal
  - Limited lifetime

## AWS Global Accelerator
* Scenario:
  - deployed a global app and have global users who want to access it directly
  - they go over the public internet which can add a lot of latency due to hops
  - we wish to go as fast as possible through the aws network to minimize latency
* Uses anycast IP
* Leverage the AWS internal network to route to your app
* 2 Anycast IP are created for oyu application
* The anycast IP send traffic directly to Edge Locations
* The Edge Locations send the traffic to your application through AWS internal network
* Work with EIP, EC2 Instances, ALB, NLB (Public or Private)
* Consistent Performance
  - intelligente routing to lowest latency and fast regional failover
  - no issue with client cache (because the ip doesn't change)
  - internal to aws network
* Health Check
  - Global Accelerator performs a health check of your applications
  - helps make your app global (failover less than 1 minute for unhealthy)
  - great for disaster recovery (thanks to the health checks)
* Security
  - only 2 external IPs need to be whitelisted
  - DDoS protection thanks to AWS Shield

## AWS Global Accelerator vs CloudFront
* They both use the AWS global network and its edge locations around the world
* both integrate with AWS Shield for DDoS protection
* CloudFront
  - improves performance for both cacheable content (images and videos)
  - Dynamic content (Such as API acceleration and dynamic site deliver)
  - Contetn iss served at the Edge
* Global Accelerator
  - improves performance for a wide range of applications over TCP and UDP
  - proxying packets at the edge to applications running in one or more AWS Regions
  - good fit for non-HTTP use cases, such as gaming (UDP) IoT (MQTT), or VoIP
  - good for HTTP use cases that require static IP addresses
  - good for HTTP use cases that require deterministic, fast regional failover
