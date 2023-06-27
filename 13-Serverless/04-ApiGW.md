 # AWS API Gateway

 * scenario: want to have client invoking our Lambda
  * can request lambda directly => needs IAM permissions
  * can use app load balancer (with http endpoint)
  * instead use:
    * use *api gw* => proxy requests to Lambda

* features:
  * Lambda + API GW => no infra to manage
  * support for websocket protocol
  * handle API versioning (v1, v2)
  * handle different environments (dev, test, prod, etc)
  * handle security (authentication and authorization)
  * create API keys, handle request throttling
  * Swagger / Open API import to quickly define APIs
  * transform and validate requests and responses
  * generate SDK and API specifications
  * cache API responses

## API GW Integrations - High Level

* Lambda function
  * invoke lambda
  * easy way to expose REST API backed by AWS Lambda
* HTTP
  * expose HTTP endpoints in the backend
  * eg: internal http API on-prem, app Lambda
    * why? add rate limit, caching, user authentications, API keys, etc
* AWS service
  * expose any AWS API through the API Gateway
  * eg: start an AWS Step function workflow, post message to sqs, etc
    * why? add authentication, deploy publicly, rate control, etc

## API Gateway - endpoint types

* edge-optimized (default):
  * for global clients
  * requests are routed thru the CloudFront edge locations (improves latency)
  * api gw still lives in only one region
* regional:
  * for clients within the same region
  * could manually combine with CloudFront == more control over caching and distribution
* private:
  * can only be accessed from within VPC using an interface VPC endpoint (ENI)
  * use a resource policy to define access

## API Gateway - Security

* user authentication through:
  * IAM roles (useful for internal apps)
  * Cognito: (identity for external users - eg: mobile users)
  * custom authroizer (your own logic - lambda)
* custom domain name https security through integration with AWS ACM (cert manager)
  * if using edge-optimized endpoint, certificate *must* be in *us-east-1*
  * if using regional endpoint, cert must be in API Gateway region
  * must set up a CNAME or A-alias record in R53
