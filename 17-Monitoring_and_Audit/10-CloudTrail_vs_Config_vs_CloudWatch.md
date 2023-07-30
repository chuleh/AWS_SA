# CloudTrail vs CloudWatch vs Config

* CloudWatch
  * performance monitoring (metrics, CPU, network, etc) && dashboards
  * Events && Alerting
  * log aggregation && Analysis
* CloudTrail
  * record api calls made within account by everyone
  * can define trails for specific resources
  * global service
* Config
  * record config changes
  * evaluate resources against compliance rules
  * get timeline of changes and compliance
* LoadBalancer ex
  * CloudWatch
    * monitoring incoming connections metric
    * visualize error codes as % over time 
    * make a dashboard to get an idea of your LB performance
  * Config
    * track SG rule for the LB
    * track config changes for the LB (ssl certs?)
    * ensure SSL cert is always assigned to the LB
  * CloudTrail
    * track who made changes to the LB with API calls

