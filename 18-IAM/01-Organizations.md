 # Organizations

 * global service
 * allows to manage multiple accounts
 * main account is management account
   * other accounts are member accounts
   * member accounts can only be part of one organization
* consolidated billing across all accounts - single payment method
* pricing benefits from aggregated usage (volume discount for EC2, S3, etc)
* shared reserved instances and savings plan discounts across all accounts
* API is available too automate account creation
* advantages:
  * multiaccount vs one account multi vpc
  * use tagging standards for billing purposes
  * enable CloudTrail on all accounts => send logs to central S3 account
  * send CloudWatch Logs to central logging account
  * enable Cross Account Roles for Admin purposes
* Security: Service Control Policies (SCP)
  * IAM policies applied to OU or Accounts to restrict Users and Policies
  * must have and explicit allow (denies all by default)
  * *does not* apply to management account (full admin)
  * SCP usually attached to Root OU


## Organizational Units (OU)

* sub orgs within accounts
* ex:
  * business unit
    * Management account
      * Sales OU
        * Sales account 1
        * Sales account 2
      * Retail OU
        * retail account 1
        * retail account 2
      * Finance OU
        * finance account 1
        * finance account 2
  * Environmental lifecycle
    * Management account
      * PROD OU
        * PROD account 1
        * PROD account 2
      * DEV OU
        * DEV account 1
        * DEV account 2
      * TEST OU
        * TEST account 1
        * TEST account 2

# SCP EBlocklist and Allowlist strategies

* either Allow * and deny on resource basis *OR* allow on resource basis and deny rest


## Good To Know
* explicit *DENY* overrides allow
* policies respect hierarchy
  * Account in an OU can have its access revoked to a resource even if its own policy allows it
  * ex
    * root account
    * mgmt account
      * prod OU => DenyRedshift
        * Account A => AuthorizedRedshift
        * cannot access even if allowed due to OU
