
# AWS Security Automation Pipeline

An automated cloud security monitoring and response system built with native AWS security services. When a threat is detected, the pipeline automatically responds — no manual intervention required.

## Architecture

**GuardDuty → EventBridge → Lambda → SNS**

| Service | Role |
|---|---|
| AWS GuardDuty | Continuously monitors AWS environment for threats using CloudTrail logs, VPC Flow Logs, and DNS logs |
| AWS CloudTrail | Captures all API activity across the AWS account for audit and forensic purposes |
| AWS EventBridge | Routes GuardDuty findings to the Lambda function via event pattern matching |
| AWS Lambda | Executes automated response — parses the finding, disables compromised IAM credentials, sends alert |
| AWS SNS | Delivers email notification with full finding details and confirmation of automated action |

## What It Does

1. GuardDuty detects suspicious activity in the AWS account
2. EventBridge catches the finding event and triggers the Lambda function
3. Lambda identifies the compromised IAM user from the finding details
4. Lambda automatically disables all access keys for that user
5. SNS sends an email alert with finding type, severity, and confirmation of the automated response

## Automated Response Demo

**Finding detected:**
`UnauthorizedAccess:IAMUser/ConsoleLoginSuccess.B` — Severity 5

**Automated action taken:**
`IAM user 'test-compromised-user' access keys disabled`

No manual intervention. End-to-end response time under 1 minute.

## Screenshots

### GuardDuty Findings
<p align="center">
<img src="https://i.imgur.com/TKnzSV1.png" height="85%" width="85%" alt="GuardDuty Findings"/>
</p>


### EventBridge Rule
<p align="center">
<img src="https://i.imgur.com/xQHcDmd.png" height="85%" width="85%" alt="EventBridge Rule"/>
</p>



### CloudWatch Execution Logs
<p align="center">
<img src="https://i.imgur.com/GW63wFG.png" height="85%" width="85%" alt="CloudWatch Execution Logs"/>
</p>


> Note: The Lambda monitor shows a throttle spike caused by the GuardDuty sample findings generator firing 400+ simultaneous findings. Individual finding responses execute at 100% success rate as confirmed in CloudWatch logs above.

### IAM User — Access Keys Disabled
<p align="center">
<img src="https://i.imgur.com/Msxwylz.png" height="85%" width="85%" alt="IAM User — Access Keys Disabled"/>
</p>



### SNS Alert Email
<p align="center">
<img src="https://i.imgur.com/xyR16ti.png" height="85%" width="85%" alt="SNS Alert Email"/>
</p>



## Tech Stack

- AWS GuardDuty
- AWS CloudTrail
- AWS EventBridge
- AWS Lambda (Python 3.12)
- AWS SNS
- AWS IAM

## Key Concepts Demonstrated

- Cloud-native threat detection using AWS managed services
- Event-driven security automation
- Automated IAM incident response
- Least privilege IAM role configuration
- CloudWatch observability for Lambda functions

## Setup

### Prerequisites
- AWS account with Free Tier active
- IAM user with appropriate permissions (do not use root)
- MFA enabled on all accounts

### Services to Enable
1. Enable GuardDuty in your target region
2. Create a CloudTrail trail with S3 logging
3. Create an SNS topic and confirm email subscription
4. Deploy the Lambda function with the IAM response role
5. Create an EventBridge rule targeting GuardDuty findings

### Lambda Environment Variables
| Variable | Description |
|---|---|
| SNS_TOPIC_ARN | ARN of your SNS topic for alert delivery |

## Author

Gerron Cudjoe — Aspiring Cloud Security Engineer  
[LinkedIn](https://www.linkedin.com/in/gerron-cudjoe) | [GitHub](https://github.com/Gerronc)
