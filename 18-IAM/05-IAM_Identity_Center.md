# AWS IAM Identity Center

* succesor to AWS SSO
* one login (single sign-on) for all your
  * AWS Accounts in AWS Orgs
  * business cloud apps (Saleforce, box, microsoft 365)
  * SAML2.0-enabled apps
  * EC2 windows instances
* identity providers
  * built-in identity store in IAM identity Center
  * 3rd party: ActiveDirectory (AD), OneLogin, Okta, etc
* permissions
  * top level: AWS Organization
  * case: 2 devs inside a *group* in *Identity Center* inside *mgmt account*
  * create *PermissionSet* and associate it to an OU
  * assing *PermissionSet* to *group*
  * can create another RO PermissionSet and assign it to group (prod for ex)

## Fine-grained permissions and assignments

* multi-account permissions
  * manage access across AWS accounts in your AWS Org
  * PermissionSets - a collection of one or more IAM policies assigned to users and groups to define AWS access
* application assignments
  * SSO access to many SAML2.0 business apps (Saleforce, Box, microsoft 365, etc)
  * provide required URLS, certificates and metadata
* ABAC (Attribute-Based Access Control)
  * Fine-grained permissions based on attributes stored in IAM Identity Center Store
  * ex: cost center, title, locale, etc
  * use case:
    * define permissions once, then modify AWS access by changing the attributes

