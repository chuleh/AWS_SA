# AWS Control Tower

* uses AWS organizations to create accounts
* easy way to set up and govern a secure and compliant multi-account aws environment based on best practices
* benefits:
  * automate set up ov environment in a few clicks
  * automate ongoing policy management using guardrails
  * detect policy violations and remediate them
  * monitor compliance thru an interactive dashboard
* GuardRails
  * provides ongoing governance for your Control Tower environments (AWS accounts)
  * 2 flavours
    * preventive guardrails - using SCPS (restric regions across all accounts)
    * detective guardrails - detect non compliance (usinh AWS Config)
      * ex: identify untagged resources
    * triggers sns topic if non-compliant
    * can invoke Lambda to remediate
