# AWS WordPress Instance – Setup & Monitoring Guide

This guide walks through the automation and monitoring of a scalable WordPress deployment on AWS.

## 1. Deploy WordPress Using CloudFormation

1.	Open the AWS console , go to cloudformation , click on create cloudformation stack
2.	Select the sample template of wordpress blog and click next
3.	Give the stack name, DBPassword, DbRootPassword, DBUser and select the instance type and key pair , click on next
4.	Keep the default settings and click next
5.	Review your data and click on submit , your cloudformation stack is created

*See:*
![CloudFormation Stack](../architecture/screenshots/Screenshot-2024-06-06-212604.jpg)

---

## 2. Create Custom AMI

1.	Go to EC2 dashboard and click on instances
2.	You will see the instance created by the stack this is your live wordpress instance
3.	Select the instance and click on actions>images and templates > create an image
4.	Give the image name , description , instance  volume 
5.	Click on create image , your AMI is created

*See:*
![AMI Creation Output](../architecture/screenshots/Screenshot-2024-06-06-212715.jpg)

---

## 3. Configure Auto Scaling Group

1.	Go to EC2 dashboard , launch templates and click on create launch template
2.	Give the template name , description 
3.	Under Application and OS images go to my AMI’s and select the AMI created before 
4.	Select the type of instance, security groups, key pairs and storage
5.	Under additional settings and subheading user details add the following line of code
     yum update -y
     yum install httpd -y
     cd /var/www/html
     echo "WEB SERVER" > index.html
     service httpd start
     chkconfig httpd on

6.	Click on create launch template
7.	Go to auto scaling groups in EC2 dashboard
8.	Click on create auto scaling groups, give the name and select the created launch template and click next
9.	Select the availability zones and subnets and click on next
10.	Keep the default settings click on next
11.	Select the desired capacity, min desired capacity, max desired capacity and under automatic scaling select the target tracking scaling policy and click on next
12.	Add notifications and click on next
13.	Add tags and click on next
14.	Review the data and click on create auto scaling group, you have created an auto scaling group

---

## 4. Lambda Function Setup

1.	Go to EC2 and select the desired instance , under tags give a key and value (ex: project and uat)
2.	Go  to IAM , create a policy , give the policy name and give EC2fullaccess and cloudwatchfullaccess click on create policy
3.	Go to IAM roles , create role , under services choose Lambda  and click next
4.	Search the created policy and select it and click next
5.	Give the role name , review the data and click on create role
6.	Go to lambda service and click on create function
7.	Select author from scratch then give the function name and choose the runtime as python , keep the default architecture and click on create function
8.	Under code write a python function to start and stop EC2 instance and deploy the code
9.	Under general configuration > environment variables give the key and value pair of the wordpress instance
10.	Test the code to see if the instance is stopping when the code is run , if excecuted successfully message is shown your lambda function to stop and start EC2 is created
11.	Go to Amazon Eventbridge and select create rule 
12.	Give the rule name (STARTEC2) and description , select the rule type as schedule and click on create rule
13.	Give a cron expression (it is a combination of min hour day month week year) as to when u want the instance to start in our case it is 9am everyday of the month every week any year , verify the next triggers to match according to our schedule and click on next
14.	Select the target as lambda function and then select the created lamabda function
15.	In the additional settings under configure target input select constant(JSON text) write the code 
{
“action”: “start
}
16.	Click on next 
17.	Review and click on create rule , you have created a rule to automatically start the  wordpress instance everyday at 9am 
18.	Similarly u can create a rule to stop the wordpress insatance everyday at 6pm by changing the cron expression to 6pm everyday and in the JSON test replace start with stop
*See:*
![Lambda Function](../architecture/screenshots/auto-start-and-stop-EC2.jpg)

---

## 5. Health Checks Configuration

1.	Open your AWS console and go to route53
2.	Click on health checks , create health checks and select specify endpoint by as IP addresses and select protocol as TCP and Port as 80
3.	Under additional settings select request interval as standard , failure threshold as default and check the box of latency graph and click next
4.	Click on create alarms as yes and then create a SNS topic by giving name and 
Email-id this will give u alert emails about the health checks
5.	Click on create health check
6.	You have successfully created a monitoring for the instance using route53’s health check feature

*See:*
![Health Check](../architecture/screenshots/new-health-check.jpg)

## 6. Verify EC2 Instances

- Review running and scaling EC2 instances in the AWS Console.

*See:*
![EC2 Instances](../architecture/screenshots/ec2-instances_1.jpg)

---

## 7. Summary & Best Practices

- This deployment achieves 99.9% uptime, fast instance launches, and proactive health monitoring.
- All steps, screenshots, and outputs are included for reference and reproducibility.

---



