# Software updates offloading

* have an application running on EC2 that distributes software updates once in a while
  * when new software is out
    * gets lots of requests
    * distributed in mass over the network
    * very costly
    * want to:
      * not change app
      * reduce cost
      * reduce CPU usage

* application state
 * ALB with 3 AZs behind and several M5
 * software stored on EFS

* solution
  * cloudfront in front of ALB
  * why?
    * no changes to architecture
    * will cache software updates at the edge
    * sw update are not dynamic, they're static
    * CloudFront is serverless == will scale || EC2 instances won't
    * ASG won't scale as much == save in EC2 && network cost
    
* CloudFront easiest way to make an app scalable and cheaper
