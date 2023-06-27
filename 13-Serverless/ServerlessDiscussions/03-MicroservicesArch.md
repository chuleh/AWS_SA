# Microservices architecture -- not fully serverless but still

* many services interact with each other using REST API
* each architecture for each microservice may vary in form and shape
* want a microservice architecture to have a leaner development cycle for each service

* proposed solutions (several available):
  * users interacts with app via HTTPS
    * service1.example.com => ALB <==> ECS <==> DynamoDB
    * service2.example.com => Api Gw <==> Lambda <==> elasticache
    * service3.example.com => ELB <==> EC2 ASG <==> RDS
  * Lambda from service2 may interact with ELB from service1
  * EC2 from service3 may interact with API GW from service2
  * all DNS queries on R53

* Discussion on microservices
  * free to design each ms the way you want
  * 2 patterns:
    * synchronous patterns:
      * api gw / LB
    * asynchronous patterns:
      * sqs / kinesis / sns / lambda
      * basically doesn't care about response
  * challenges with ms:
    * repeated overhead for creating each new ms
    * issues with optimizing server density/utilization
    * complexity of running multiple versions of multiple ms simultaneously
    * proliferation of client-side code requirements to integrate with  many separate services
  * some of the challenges are solved by serverless
    * api gw / lambda scale automatically and you pay per usage
    * easily clone API / reproduce environments
    * generate client SDK through Swagger integration for the API Gateway
