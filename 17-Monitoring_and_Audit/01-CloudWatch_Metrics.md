# CloudWatch Metrics

* provides metrics for *every* service in AWS
* metric is a variable to monitor (CPUUtilization, NetworkIn, etc)
* metrics belong to *namespaces*
* dimension is an attribute of a metric (instance id, environment, etc)
* up to 30 dimensions per metric
* metrics are time-based == must have *timestamps*
* can create a CloudWatch dashboard of metrics
* can create CloudWatch *custom metrics* (RAM for ex)


## CloudWatch Metric Streams

* continually stream CW metrics to destination of choice
* near-real-time delivery and low latency
* destinations:
  * KDF (and another destination)
    * can be sent to S3 => analyzed with Athena
    * sent to RedShift
    * sent to OpenSearch
  * 3rd party: datadog, dynatrace, new relic, splunk, sumo logic, etc
* option to filter metrics to only stream a subset of them
