# Caching Strategies

* take into account
  * caching
  * TTL
  * network
  * computation
  * cost
  * latency

## Common pattern integrations

* client -> CloudFront -> API Gateway -> AppLogic: EC2/Lambda -> Redis/Memcached/DAX <=> Database
         -> CloudFront Edge -> S3
  * CloudFront: quick responde but at edge so it could be outadted => add TTL
    * adding TTL pulls new data from EC2 => balance between caching at edge and caching at app logic
  * API Gateway: doesn't need CloudFront due to caching capabilites
    * regional cache since it's a regional service
    * bit more of lag between client and response
  * AppLogic/Lambda
    * doesn't do caching per se => need to add Redis/Memcached/DAX to store *frequent* queries
    * DB doesn't get hit that often
* the more you move between the line the bigger the cost
