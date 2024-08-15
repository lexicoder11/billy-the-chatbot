# billy-the-chatbot

## Introduction
### Project Overview
This project is a medical customer service chatbot designed to assist patients with common inquiries and appointment scheduling. The chatbot leverages AWS services for natural language understanding, serverless functions, and data storage.

### Technology Stack
- **Amazon Lex**: For natural language understanding and chatbot interactions.
- **AWS Lambda**: For executing backend logic.
- **Amazon DynamoDB** (Optional): For persistent data storage.
- **AWS IAM**: For managing permissions and roles.
- **Amazon CloudWatch**: For logging and monitoring.

## Architecture
### System Architecture Diagram
![Architecture Diagram](link_to_diagram.png)

### Component Descriptions
- **Amazon Lex Bot**: Manages user interactions and understands user intent.
- **AWS Lambda Functions**: Executes the business logic based on the intents.
- **Amazon DynamoDB** (Optional): Stores user data and appointment details.

## Setup and Configuration
### Prerequisites
- AWS account
- Basic knowledge of AWS services
- IAM roles and policies setup

### AWS Setup
#### IAM Roles and Permissions
1. Create IAM roles for Lex and Lambda with the necessary permissions.
2. Attach policies for DynamoDB access (if using DynamoDB).

### Creating the Lex Bot
#### Bot Configuration
1. Go to Amazon Lex console.
2. Create a new bot with the desired name and language.
3. Configure basic settings like session timeout and IAM role.

#### Defining Intents and Sample Utterances
1. Create intents like `BookAppointment`, `GetMedicalInfo`.
2. Add sample utterances for each intent.

#### Adding and Configuring Slots
1. Define slots like `AppointmentDate`, `ServiceType`.
2. Configure prompts for slot elicitation.

#### Setting Up Fulfillment
1. Configure the Lambda function as the fulfillment mechanism.
2. Test the bot's response with sample interactions.

## Lambda Functions
### Creating Lambda Functions
1. Create a new Lambda function in the AWS Lambda console.
2. Choose the appropriate runtime (e.g., Node.js, Python).

### Code Implementation
```javascript
const AWS = require('aws-sdk');
const dynamoDB = new AWS.DynamoDB.DocumentClient();

exports.handler = async (event) => {
    const intentName = event.currentIntent.name;

    if (intentName === 'BookAppointment') {
        const userId = event.userId;
        const appointmentDate = event.currentIntent.slots.AppointmentDate;
        const params = {
            TableName: 'Appointments',
            Item: {
                UserId: userId,
                AppointmentDate: appointmentDate,
                // Add other attributes as needed
            },
        };

        await dynamoDB.put(params).promise();

        return {
            dialogAction: {
                type: 'Close',
                fulfillmentState: 'Fulfilled',
                message: {
                    contentType: 'PlainText',
                    content: 'Your appointment has been booked for ' + appointmentDate,
                },
            },
        };
    }
    // Handle other intents
};
