# Amazon Inspector

* *only for* EC2 instances, Container Images, Lambda Functions
* for automated security assesment
* for EC2 instances
  * leveraging AWS SSM agent
  * analyze against unintended network accesibility
  * analyze running OS against known vulnerabilities
* for container images pushe to ECR
  * assesment of container images as they are pushed
* for lambda functions
  * identifies software vulnerabilities in function code and package deps
  * assesment of functions as they are deployed
* reporting & integration with AWS Security Hub
* send findings to EventBridge
* continous scanning of infra only when needed
  * package vulnerabilities (EC2, ECR, Lambda) - db of CVE
  * network reachability (EC2)
* a risk score is associated with all vulns for priorization
