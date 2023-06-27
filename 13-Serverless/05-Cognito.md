# Amazon Cognito

* give users a identity to interact with our web or mobile app outside of our aws account
* cognito user pools:
  * sign in functionality for app users
  * integrate with API GW & ALB
* cognito identity pools:
  * provide AWS credentials to users so they can access AWS resources directly
  * integrate with cognito user pools as an identity provider

## Amazon Cognito User Pools (CUP) - User features

* create a serverless database of user for your web && mobile apps
* simple login: username (or email) + password combination
* password reset
* email && phone verification
* MFA
* federated identities: users from FB, Google, SAML
* integrates with API GW
  * user retrieves token from cognito user pools
  * pass token to api gw
  * if ok translates into a user identity
  * passed to lambda on backend
* integration with ALB
  * app connects to cognito user pool
  * passed onto ALB
  * if login is true pass to backend with additional headers

## Cognito identity Pools (federated identities)

* get identities for "users" so they obtain temporary aws credentials
* users source can be Cognito user pools or 3rd party logins
* users can access the AWS service directly or through api gw
* user policies applied to the credentials are defined in cognito
* can be customized based on the *user_id* for fine grained control
* want to have default IAM role fro authenticated or guest users
