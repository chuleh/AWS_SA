# Invoking Lambda from RDS & Aurora

* invoke lambda functions from within your DB instance
* allows to process *data events* from within a DB
* supported fro RDS for PSQL and Aurora MySQL
* *must* allow outbound traffic to your Lambda from within your DB instance (public, nat gw, vpc endpoints)
* DB instance must have the required permissions to invoke Lambda (Lambda resource-based policy && IAM policy)

## RDS even notifications

* notifications that tells info about the DB instance itself (creation, stop, start)
* different than *invoking* lambda
  * you have *no information* about the data itself
* can subscribe to the following categories
  * db instance
  * db snapshot
  * db parameter group
  * db security group
  * rds proxy
  * custom engine version
* near real-time events (up to 5 min)
* send notifications to SNS or subscribe to events using Event Bridge
