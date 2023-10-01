# AWS Backup

* fully managed service
* centrally manage and automate backups across AWS services
* no custom scripts or manual processes
* supported services
  * EC2 / EBS
  * S3
  * RDS (all engines) / Aurora / DynamoDB
  * DocumentDB / Neptune
  * EFS / FSx (Lustre && Windows File Server)
  * Storage Gateway (Volume Gateway)
* supports:
  * cross-region backups
  * cross-account backups
  * PITR for supported services (ex: Aurora)
  * on-demand and scheduled backups
  * tag based backup policies
* backup policies (backup plans)
  * backup frequency: every 12 hours, daily, weekly, monthly, cron expression
  * backup window
  * transition to cold storage (never, days, weeks, months, years)
  * retention period (always, days, weeks, months, years)
* all sent to S3 specific bucket

## Backup Vault Lock

* enforce a WORM (Write Once Read Many) state for all the backups that you store in your AWS vault backup
* additional layer of defense to protect backups against
  * inadvertent or malicious delete operations
  * updates that shorten or alter retetion periods
* even root user *cannot delete* backups when enabled
