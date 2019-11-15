#### By default we cannot monitor menory metrics on Ec2 instances. But by using a custom metric we can. In this document I walk through the process of settings custom metrics

#### To install the required packages on Amazon Linux 2 and Amazon Linux AMI. With some versions of Linux, you must install additional Perl modules before you can use the monitoring scripts. 
```
yum install -y perl-Switch perl-DateTime perl-Sys-Syslog perl-LWP-Protocol-https perl-Digest-SHA.x86_64
```

#### The following steps show you how to download, uncompress, and configure the CloudWatch Monitoring Scripts on an EC2 Linux instance.

```
To download, install, and configure the monitoring scripts :
curl https://aws-cloudwatch.s3.amazonaws.com/downloads/CloudWatchMonitoringScripts-1.2.2.zip -O

unzip CloudWatchMonitoringScripts-1.2.2.zip

cd aws-scripts-mon
```

#### You have to provide an IAM role or awscreds.conf file. Otherwise, you must provide credentials using the --aws-access-key-id and --aws-secret-key parameters for these commands. 

#### The following example performs a simple test run without posting data to CloudWatch.

```
[root@ip-172-31-15-12 aws-scripts-mon]# ./mon-put-instance-data.pl --mem-util --verify --verbose
MemoryUtilization: 11.6693074103636 (Percent)
No credential methods are specified. Trying default IAM role.
Using IAM role <Admin_access>
Endpoint: https://monitoring.ap-south-1.amazonaws.com
Payload: {"MetricData":[{"Timestamp":1573802311,"Dimensions":[{"Value":"i-03c727b75fc014fd6","Name":"InstanceId"}],"Value":11.6693074103636,"Unit":"Percent","MetricName":"MemoryUtilization"}],"Namespace":"System/Linux","__type":"com.amazonaws.cloudwatch.v2010_08_01#PutMetricDataInput"}

Verification completed successfully. No actual metrics sent to CloudWatch.
````

#### The following example collects all available memory metrics and sends them to CloudWatch, counting cache and buffer memory as used 

````
[root@ip-172-31-15-12 aws-scripts-mon]# ./mon-put-instance-data.pl --mem-used-incl-cache-buff --mem-util --mem-used --mem-avail

Successfully reported metrics to CloudWatch. Reference Id: a6b0416f-8385-4bfc-8236-bcd7ccade1f5
```
#### To set a cron schedule for metrics reported to CloudWatch, start editing the crontab using the crontab -e command. Add the following command to report memory and disk space utilization to CloudWatch every five minutes: 

```
*/5 * * * * /home/ec2-user/aws-scripts-mon/mon-put-instance-data.pl --mem-used-incl-cache-buff --mem-util --disk-space-util --disk-path=/ --from-cron
```

