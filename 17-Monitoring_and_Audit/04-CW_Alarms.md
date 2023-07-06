# CloudWatch Alarms

* alarms are used to trigger notifications for any metric
* various options (sampling, %, max, min, etc)
* Alarm States:
  * OK
  * INSUFFICIENT_DATA
  * ALARM
* Period
  * lenght of time in seconds to evaluate the metric
  * high resolutions custom metric: 10 sec, 30 secs or multiples of 60 sec

## CloudWatch Alarms Targets

* Stop, Terminate, Reboot or Recover an EC2 instance
* triger autoscaling action (scale out - scale in)
* send notification to SNS (from which you can do pretty much anything)

## CloudWatch Alarms - Composite Alarms

* CloudWatch alarms are on a single metric
* *Composite Alarms* are monitoring the state of multiple other alarms
* AND and OR conditions
* helpgul toe reduce "alarm noise" by creating complex composite alarms

## EC2 instance recovery

* status check:
  * instance status: check the EC2 VM
  * system status: check the underlying hardware
* define CloudWatch alarm
* if failed => *StatusCheckFailed_System* => Trigger EC2 instance recovery
  * recovery: move EC2 instance host to another with same: Private, Public, EIP, metadata, placement group

## CloudWatch Alarm good to know

* alarms can be created based on CloudWatch Logs Metrics Filters
* To test alarms and notifications, set the alarm state to Alarm using CLI
