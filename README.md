# AWS Cloud Security monitoring 


This report outlines the process of configuring AWS services to detect and alert on potential attacker activity through API monitoring. The setup begins with the creation of an AWS account and the configuration of Amazon Simple Notification Service (SNS) to send email alerts. An SNS topic and subscription are created to notify users of suspicious actions.

Next, AWS CloudTrail is configured to log all API activity within the AWS environment, enabling the identification of potentially malicious behavior. During setup, CloudTrail is integrated with the previously created SNS topic to automate alerting.

Following this, a rule is created in Amazon EventBridge using a custom JSON pattern to monitor specific API calls, particularly the GetCallerIdentity action from AWS STS. The rule targets the SNS topic to ensure real-time alerting.

Finally, the setup is tested by simulating attacker behavior through AWS CLI on a virtual machine. Upon execution of the sts get-caller-identity command, an email alert is successfully triggered, confirming that the detection and notification pipeline is functioning as intended.


<h2>Project walk-through:</h2>

<p align="center">
Set up an AWS account : <br/>

![image alt](https://github.com/Samuel-James971/Cloud-Home-Lab/blob/main/1.png?raw=true)
<br />
<br />
 Set up simple notification service so that you can get an email anytime attacker activity is detected. Here I have created a topic named APICallAlert1:  <br/>
![image alt](https://github.com/Samuel-James971/Cloud-Home-Lab/blob/main/2.png?raw=true)
<br />
<br />
The next step is to create a subscription for this topic. Select the protocol email and emter the email address where you would like to recive the alerts. Now SNS has been set up to send email alerts: <br/>
![image alt](https://github.com/Samuel-James971/Cloud-Home-Lab/blob/main/3.png?raw=true)
<br />
<br />
The next step is to set up cloud trail, this will let us log everyhting that takes place in our AWS account. By logging the activity it will be able to detect any maliicous activity whcih will then be notified by SNS:   <br/>
![image alt](https://github.com/Samuel-James971/Cloud-Home-Lab/blob/main/4.png?raw=true)
<br />
<br />
Wgen setting up cloud trail, enable SNS. I have chosen the original SNS topic that I set up earlier:  <br/>
![image alt](https://github.com/Samuel-James971/Cloud-Home-Lab/blob/main/5.png?raw=true)
<br />
<br />
Once the cloud trail has been set up the status should read as logging:  <br/>
![image alt](https://github.com/Samuel-James971/Cloud-Home-Lab/blob/main/6.png?raw=true)
<br />
<br />

The next step is to set up a rule in Amazon event bridge:  <br/>
![image alt](https://github.com/Samuel-James971/Cloud-Home-Lab/blob/main/7.png?raw=true)
<br />
<br />
In the creation method, choose Custom pattern (JSON editor). Within the custom JSON, the "source" field specifies the AWS resource— in this case, it's aws.sts. The "detail-type" should be set to AWS API Call via CloudTrail, which indicates that the event type being captured is an API call recorded by CloudTrail.

The "eventSource" field confirms the event is related to the STS (Security Token Service), which is used for identity and access management. Finally, the "eventName" identifies the specific API call being made to the STS service— in this case, it is GetCallerIdentity.":  <br/>
![image alt](https://github.com/Samuel-James971/Cloud-Home-Lab/blob/main/8.png?raw=true)
<br />
<br /> 
In the next step which is select targets you want to select SNS topic and search for the one that you created, which in this case is APICallAlert1. :  <br/>
![image alt](https://github.com/Samuel-James971/Cloud-Home-Lab/blob/main/9.png?raw=true)
<br />
<br />
 
Here you can see the sataus is enabled meaning the amazon event bridge rule has been succesfully set up:  <br/>
![image alt](https://github.com/Samuel-James971/Cloud-Home-Lab/blob/main/10.png?raw=true)
<br />
<br />
To run the attacker commands, use a VM Firstly iin Powershell I have entered the command "aws configure". This command sets up the CLI to interact with the AWS console. When entering this it should prompt you yo enter you AWS access key and then your AWS secret access key. After entering these it will prompt you to enter your region name which in this case is "eu-north-1". Then it will ask you for the default output format, here I have entered JSON.
Now you can run the command sts get-caller-identity which should create an email alert.
![image alt](https://github.com/Samuel-James971/Cloud-Home-Lab/blob/main/15.png?raw=true)
<br />
Here you can see the email alert has been triggered, containing the log that has been generated from that alert.
![image alt](https://github.com/Samuel-James971/Cloud-Home-Lab/blob/main/16.png?raw=true)





<!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
--!>
