# AWS Secrets Manager

* newer service meant for storing secrets
* capability to force rotation of secrets every X days
* automate generation of secrets on rotation (uses Lambda)
* integration with RDS (MySQL, PSQL, Aurora)
* can be encrypted using KMS

## Multi-region secrets

* replicate secrets across multiple aws regions
* Secrets Manager keeps read replicas in sync with the primary secret
* ability to promote a read replica secret to a standalone secret
* use case:
  * multi-region apps
  * disaster recovery strategies
  * multi-region db
