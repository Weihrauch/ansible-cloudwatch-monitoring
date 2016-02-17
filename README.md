Cloudwtach monitoring
=========

This role is intended to automaticaly install and configure the AWS script to report memory and disk usage to cloudwatch console.
 
Script usage and documentation can be found on AWS doc : [here](http://docs.aws.amazon.com/AmazonCloudWatch/latest/DeveloperGuide/mon-scripts.html)

The script is started through a wrapper which set AWS_EC2CW_META_DATA to /var/run/aws-mon. This variable is used by AWS script to cache fetched instance-id. As /var/run is cleared on startup you won't have an invalid instance-id issue in the case of AMI build.

Requirements
------------

Servers targeted by this role must be EC2 instance and should have IAM profile which allows them to perform the following operations :
   * cloudwatch:PutMetricData
   * cloudwatch:GetMetricStatistics
   * cloudwatch:ListMetrics
   * ec2:DescribeTags

Role Variables
--------------

- cw_script_download_url : URL to download AWS cloudwatch scripts
	* Type : string
	* Default : http://aws-cloudwatch.s3.amazonaws.com/downloads/CloudWatchMonitoringScripts-1.2.1.zip

- cw_install_dir : Folder where scripts will be installed
	* Type : string
	* Default : /opt/aws-scripts-mon/

- cw_options : Arguments used when calling cloudwatch script
	* Type : string
	* Default : --disk-space-util  --disk-path=/ --disk-space-used --disk-space-avail --swap-util --swap-used --mem-util --mem-used --mem-avail

- cw_cron_minute : Frequency (in minutes) of data reporting to cloudwatch. Must be written with crond syntax. You may want to set it to "*" if you are using detailled monitoring for your EC2 instances, to send monitoring data every 5 minutes.
	* Type : string
	* Default : "*/5"

Dependencies
------------


Example Playbook
----------------

    - hosts: servers
      roles:
         - { role: ansible-cloudwatch-monitoring, cw_cron_minute: "*/5" }

License
-------

BSD

Author Information
------------------

email : pauchet.valentin at gmail.com

