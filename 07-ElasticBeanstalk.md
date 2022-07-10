# Elastic Beanstalk
* For devs
* Free but pay for underlying resources
* Managed Service
* Instance config / OS is handled by beanstalk
* Deployment strategy is configurable but managed by beanstalk
* Just the app code is responsiblity of the developer
* 3 architecture models:
  - single instance deployment => good for dev
  - LB + ASG => great for prod or preprod web apps
  - ASG only => great for non web apps in prod (workers, etc)
* 3 components:
  - application
  - application version => each deployment gets assigned a new version
  - environment name (dev, test, prod) free naming
* Your deployment application versions to environments and can promote application versions to the next environment
* Rollback feature to previous application version
* Full control over lifecycle of environments
