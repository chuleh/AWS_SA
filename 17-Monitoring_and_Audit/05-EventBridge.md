# Amazon EventBridge (formerly CW Events)

* default event bus
  * where AWS Services sends events

* schedule: cron jobs (scheduled scripts)
* react to event pattern: event rules react to a service doing something
  * IAM Root User Sign in event => send SNS topic with email notif
* trigger lambda functions, send SQS/SNS messages

## Amazon EventBridge Rules

* rules can be created from any AWS services and trigger another AWS service

## Partner Event Bus

* AWS SaaS partners (DataDog, ZenDesk)
  * sends their events into their partner event bus

## Custom Event Bus

* own custom apps send events to their own custom event bus

## Schema Registry

* JSON format
* EventBridge can analyze the events in your bus and infer the *schema*
* the schema registry allows to generate code for you app that will know in advance how data is structured in the even bus
* schema can be versioned

## Resource-based policy

* manage permissions for a specific Even Bus
  * ex: allow/deny events from another AWS account or region
  * aggregate all events from your AWS Organization in a single AWS Acct or AWS Region

## Good to know

* event buses can be accessed by other AWS accounts using resource-based policies
* you can *archive events* sent to an event bus (indef or set period) => ability to replay archived events
