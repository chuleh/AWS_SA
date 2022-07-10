# IAM

## Identity and Access Management
* users / groups / roles
* _GLOBAL_
* Policies written in JSON
* Can have MFA
* IAM has predefined policies


- user: a phsyical person - can belong to several groups
- group: contains users - can only contain users _NOT_ other groups
- roles: internal usage within AWS resources - we give the to machines

## Permissions
Each of them have _policies_ that defines what each of them can and cannot do.

- `Users` or `Groups` can be assigned JSON documents called `POLICIES`
- Policies: define permissions of the users
- One IAM _user_ per PHYSICAL PERSON
- one IAM _role_ pero application

- User can have individual policy + group policies
  Best practice: create user > add user to group and inherit policy

## Policy Structure
- Consists of:
  - _Version_: policy language version -always goes 2012-10-27-
  - _Id_: identifier for the policy -optional-
  - _Statement_: one or more individual statements -required-
    - consists of:
      - _Sid_: an identifier for the statement -optional-
      - _Effect_: whether the statement *allows* or *denies* access (Allow, Deny)
      - _Principal_: account/user/role to which this policy is applied to
      - _Action_: list of actions this policy allows or denies
      - _Resource_: list of resources to which the actions applied to
      - _Condition_: (optional) conditions for when the policy is in effect

## Password Policy
- Set a minimum password length
- Require specific character types
  - uppercase letters
  - lowercase letters
  - numbers
  - alphanumeric characters
- Allow all IAM users to change their passwords
- Password expiration (change password after a time)
- Prevent password reuse

## MFA (Multi Factor Authentication)
- MFA == password you _know_ + security device you _own_
- MFA options:
  - Virtual MFA device
    - google authenticator (phone only)
    - authy (multi device)
  - Physical MFA device
    - U2F (Universal 2nd Factor) Security Key
    - YubiKey by Yubico (3rd party)
    - Hardware Key FOB MFA device by Gemalto (3rd party)
    - Hardware Key FOB MFA device for AWS GovCloud by SurePassID (3rd party)

## Iam Roles for Services
- Used when an AWS service needs to perform actions on your behalf
  - we assign *permissions* to AWS services with *IAM Roles*

## IAM Security Tools
- IAM Credentials Report (account-level)
  - report that lists all you accounts users and the status of their various credentials
- IAM Access Advisor (user-level)
  - shows the srevice permissions granted to a user and when those services were last accesed

## IAM Guidelines && Best practices
* Don't use *root account* except for AWS account setup
* One phsyical user == one AWS user
* Assign users to groups and assign permissions to groups
* Create a strong password policy
* User and enforce use of MFA
* Create and use roles for giving permissions to AWS services
* Use Access Keys for Programatic Access (CLI/SDK)
* Audit permissions of your account with IAM Credentials Report
* Never share IAM users && Access Keys

## IAM Federation
* Big enterprises usually integrate their own repository of users with IAM
* Can login into AWS using their company credentials (eg: Active directory)
* Identity Federation uses the SAML standard

