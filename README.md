
# NBA Game Day Notifications / Sports Alerts System

## **Project Overview**

This project is an alert system that sends real-time NBA game day score notifications to subscribed users via SMS/Email. It leverages **Amazon SNS**, **AWS Lambda (Python)**, **Amazon EventBridge**, and **NBA APIs** to provide sports fans with up-to-date game information. The project demonstrates cloud computing principles and efficient notification mechanisms.
 

---
## **Features**

- Fetches live NBA game scores using an external API
- Sends formatted score updates via SMS/Email using Amazon SNS
- Scheduled automation using Amazon EventBridge
- Security-focused design with least privilege IAM roles

## **Prerequisites**

- Free [sportsdata.io](https://sportsdata.io/) account with API key
- AWS account with basic cloud understanding
- Python 3.x knowledge

---

## **Technical Architecture**

![System Architecture]
![game-day](https://github.com/user-attachments/assets/68ba5a95-6851-4cb6-a811-6b58f1ec218e)



---
## **Technologies**

- **Cloud Provider**: AWS
- **Core Services**: SNS, Lambda, EventBridge
- **External API**: SportsData.io NBA API
- **Language**: Python 3.x
- **IAM Security**: Least privilege policies
---
## **Project Structure**

```bash

game-day-notifications/

├── src/

│ └── gd_notifications.py # Lambda handler

├── policies/

│ ├── gd_sns_policy.json # SNS permissions

│ ├── gd_eventbridge_policy.json # EventBridge config

│ └── gd_lambda_policy.json # Lambda execution role

├── .gitignore

└── README.md

```

## 🛠️ Setup Instructions

### 1. Clone Repository

```bash

git clone https://github.com/sojay/game-day-notifications.git

cd game-day-notifications

```

### **2. AWS SNS Configuration**

1. **Create SNS Topic**
    
    - AWS Console → **SNS** → Create Topic
    - Type: Standard
    - Name: `gd_topic`
    - Note the Topic ARN (will need later)
        
2. **Add Subscriptions**
    - For Email:
        ```bash
		Protocol: Email
        Endpoint: your.email@domain.com
        ```

        - Confirm via confirmation email
        
### **3. IAM Policy Setup**

1. Create policy from `policies/gd_sns_policy.json`
    
```json
{
        "Version": "2012-10-17",
        "Statement": [{
            "Effect": "Allow",
            "Action": "sns:Publish",
            "Resource": "arn:aws:sns:REGION:ACCOUNT_ID:gd_topic"
        }]
    }
```
    
    - Replace `REGION` and `ACCOUNT_ID` with your values
        
1. **Attach Policies to Role**
    - Create IAM Role for Lambda
    - Attach policies:
        - `gd_sns_policy` (custom)
        - `AWSLambdaBasicExecutionRole` (AWS managed)

### **4. Lambda Deployment**

1. **Create Function**
    - Runtime: Python 3.x
    - Name: `gd_notifications`
    - Role: Select created IAM role
        
2. **Configure Function**
    - Paste code from `src/gd_notifications.py`
    - Set environment variables:
        ```bash
        NBA_API_KEY = your_sportsdata.io_key
        SNS_TOPIC_ARN = your_topic_arn
```
        
### **5. EventBridge Scheduling**

3. Create new rule
    
4. Set schedule pattern:
```bash
rate(15 minutes)  # Example: 15-min intervals
```
    
5. Add target:
    - Select `gd_notifications` Lambda function

### **6. Validation**

6. **Test Lambda**
    - Use test event in Lambda console
    - Check CloudWatch logs for errors
        
7. **Verify Notifications**
    - Check subscribed email
    - Allow 15-30 mins for first scheduled execution
