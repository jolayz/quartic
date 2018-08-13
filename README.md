# Streaming Data Filtering

## Problem
Problem Statement
Build a rule engine that will apply rules on streaming data. Your program should be able to perform followingtasks, at minimum:

Allow users to create rules on incoming data stream
Execute rules on incoming stream and show the data that violates a rule.
Incoming data stream is a tagged data stream. Each incoming data is a hashmap with following syntax

```
{ 'signal': 'ATL1', 'value': '234.98', 'value_type': 'Integer'}
{ 'signal': 'ATL2', 'value': 'HIGH', 'value_type': 'String'}
{ 'signal': 'ATL3', 'value': '23/07/2017 13:24:00.8765', 'value_type': 'Datetime'}
...
...
...
```

In general, a data unit would have three keys

signal: This key specifies the source ID of the signal. It could be any valid alphanumeric combo. ex: ATL1, ATL2, ATL3, ATL4
value: This would be the actual value of the signal. This would always be a string. ex: '234', 'HIGH', 'LOW', '23/07/2017'
value_type: This would specify how the value is to interpreted. It would be one of the following
Integer: In this case the value is interpreted to be an integer. Ex: '234' would be interpreted as 234
String: In this case the value is interpreted to be a String. Ex: 'HIGH' would be interpreted as 'HIGH'
Datetime: In this case the value is interpreted to be a Date Time.
Rules can be specified for a signal and in accordance to the value_type. Some examples of rules are:

ATL1 value should not rise above 240.00
ATL2 value should never be LOW
ATL3 should not be in future

## Requirement Assumptions
Data Payload - The sample payload json file is about 13KB. Make assumption here, the average payload is about this size.
Data Scale - In current example, we assume the data scale at the begining will be 10KB per minute.
Data Format - From the sample payload, it's well formatted JSON format. Field "signal" is always 4 characters, "value_type" are always in ["String", "Datetime", "Integer"] and always present the data type of "value" field in the row.
Data Security Classification - Assume as cenfidential.

## Architect
The architect will be built on top of AWS (Amazon Web Service). Using Kinesis Stream as data input and output. Using Kinesis Analytics as filtering engine.

Reasons:
1. Cost Effective - with AWS, I don't need to own or maintain Data Center or hosts. The cost will be smoothy and only pay for the usage.
2. Serverless - low maintanance cost. I don't even need to maintain any hosts or virtual machine. No need for security update, hardware CPU, memory and harddisk monitor, no need for setting up new hosts.
3. Easy to scale up/down - when demand change, I could easily scaling up or down, without dealing with hardware.
4. Monitor/Alarm support

## Cost Analytics
Assuming I have 10KB per minitue at the beginning. It will be 10KB * 60 * 24 * 30 = 432GB per month. The cost for the input and output Kinesis Stream will be $13 per month. Kinesis Analytics will only need 1 KPU at the beginning, would be $79.20 if we always keep it running. Consider at the beginning, we don't have too much payload, we could consider only running a few hours a day if we don't have strong requirement on real-time. The total cost could between $20 ~ $90 per monthl

## Steps to Use It

### Step 1 - Register Your Own AWS Account
Create an AWS account is FREE - https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/cfn-sign-up-for-aws.html
It could connect with your Amazon Retail account.

### Step 2 - Create Stack in CloudFormation
In this package, I will have a CloudFormation template. After you log in to your AWS console, select CloudFormation service. Press "Create Stack" and paste this template. It will create all the AWS components needed by this test in your AWS account. After test, you could delete the stack and it will clean up the resources created by it.

### Step 3 - Send a payload to Kinesis Stream




















