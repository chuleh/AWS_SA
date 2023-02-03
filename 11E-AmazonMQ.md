# AmazonMQ

* managed broker service for:
  * RabbitMQ && ActiveMQ
* when migrationg to the cloud, instead of re-engineering the app to use SQS and SNS (AWS propietary protocols) we can use AmazonMQ
  * traditional open protocols like: MTT, AMQP, STOMP, Openwire, WSS

## AmazonMQ HA

* define two brokers in different AZ
  * 1 broker is *ACTIVE* the other one is *STANDBY*
  * both brokers backed by *EFS* for failover
  * same data kept by EFS
