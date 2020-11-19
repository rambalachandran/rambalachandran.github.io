---
title:  "Automate AWS EC2 Spot Instance Creation"
image:  /assets/images/blog_posts/aws_ec2_logo.png
author: Ram Balachandran
# comments: false
# date:   2020-08-23 08:30:00 +0530
categories:
    - DevOps
    - AWS
layout: post
---
## Automate Spot Instance Creation
One of the major pain points for me to work with AWS is the time that I need to spend every day trying to provision an EC2 Spot Instance. While, it is much cheaper to use Spot instance, it takes a lot of productive time going through the same routine every day.

While there are full fledged enterprise level IT automation tools like  [ansible](https://docs.ansible.com/) which are more useful to handle systems in production, for individuals working on development and prototyping, this can be easily achieved using `boto3` package of python along with `crontab` in linux to automate this boring step. 


### Config File
The first step is to create a config file as shown below. Lets name it `config.cfg`. The file inputs are as follows.
```apacheconf
[EC2]
ami=ami-0885b1f6bd170450c
key_pair=keypair_test
max_bid=0.01
instance_count=1
instance_run_type=one-time
instance_machine_type=t2.micro
instance_interrupt_type=terminate
instance_availability_zone=us-east-1f
security_group=sec_group1
iam_role=
subnet_id= 
tag=test-spot-instance
team=me
created_by=me
application=qa
product_description=Linux/UNIX
username=ubuntu
```
Let us go through the important options here.
1. `ami` should contain the ami-id from which the instance needs to be provisioned. Here I have used the ami corresponding to Ubuntu 20.01.
2. `key_pair` is the name of the `.pem` file (`keypair_test.pem`) that will be used to tunnel into the virtual instances.
3. `max_bid` is the maximum value you are willing to pay for the instance. The higher the price, the less likely the spot instance to be terminated.
4. Then you can specify various instance related information such as `instance_count`, `instance_run_type`, `instance_machine_type` and `instance_interrupt_type`. If required, you can also specify the zone where you need the instance.
5. If you need to attach the instance to a specific security group, you can use the `security_group` where the value is the name of the security group. Similarly, we can include information of relevant `subnet_id` and `iam_role` if needed.
6. The other information such as `tag`, `created_by` and `application` are tag values that are included to the EC2 resource tag.

### Python code to create EC2 instance
The code that uses the config file to provision an EC2 spot instance is called `ec2_create.py` and is provided in this [github repository](https://github.com/rambalachandran/pyAWS). The repository also has sample config file (`config_sample.cfg`) that can be customized and used. The code can be run as follows

```sh
python ec2_create.py --config config.cfg
```
This will create the following output
```sh
Reading Config File
Spot instance request success, status: open
Waiting... None
Instance allocated ,Id:  <ec2-id>
```
Now this code can be easily scheduled for daily operation using *crontab*. This [webpage](https://phoenixnap.com/kb/set-up-cron-job-linux) provides an excellent introduction to crontab. I prefer to collect all my crontab jobs into a single folder. So for this, I created a script `create_ec2.sh` with the following code

```sh
/path/to/python /path/to/ec2_create.py --config /path/to/config.cfg
```
Save the script in a common folder which in my case is `~/crontabJobs`. Make the script executable using the following command

```sh
chmod +x ~/crontabJobs/create_ec2.sh
```

To edit crontab, type the following command
```sh
crontab -e
```

This opens the file for edit. If we need to provision the instance everyday of the month at 08:00AM we can simply add the following line
```sh
0 8 * * * /home/username/crontabJobs/create_ec2.sh >> /home/username/crontabJobs/cron.log 2>&1
```
where 
1. first 0 indicates minutes, 
2. 8 indicates hour of day, 
3. the next three stars indicate the day of the month, month of the year and day of week. 
   
If you do not have a local linux machine, you can keep a *t2.micro* instance running in AWS and perform all these tasks from that instance.

## Conclusion
The provisioning of EC2 spot instance that has lower cost can be automated using boto3 and crontab and save sometime from these boring tasks.