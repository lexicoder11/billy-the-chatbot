# Billy the Chatbot

## Introduction
### Project Overview
This project is a medical customer service chatbot designed to assist patients with booking appointments and common medical bill inquiries. The chatbot leverages AWS services for natural language understanding, serverless functions, and data storage.

### Technology Stack
- **Amazon Lex**: For natural language understanding and chatbot interactions.
- **AWS Lambda**: For executing backend logic.
- **Amazon DynamoDB** : For persistent data storage.
- **AWS IAM**: For managing permissions and roles.
- **Amazon CloudWatch**: For logging and monitoring.

## Architecture
### System Architecture Diagram
![Architecture Diagram](link_to_diagram.png)

### Component Descriptions
- **Amazon Lex Bot**: Manages user interactions and understands user intent.
- **AWS Lambda Functions**: Executes the business logic based on the intents.
- **Amazon DynamoDB**: Stores user data and appointment details.

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
1. Create intents titled `Book Appointment`,`GetMedicalBillInfo`.
2. Add sample utterances for the intent.

#### Adding and Configuring Slots
1. Define slots `ServiceType`.
2. Configure prompts for slot elicitation.

#### Setting Up Fulfillment
1. Configure the Lambda function as the fulfillment mechanism.
2. Test the bot's response with sample interactions.

## Lambda Functions
### Creating Lambda Functions
1. Create a new Lambda function in the AWS Lambda console.
2. Choose a programming language. I used Javascript.

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

exports.handler = async (event) => {
    const intentName = event.currentIntent.name;

    if (intentName === 'GetMedicalBillInfo') {
        const billQueryType = event.currentIntent.slots.BillQueryType;
        const patientID = event.currentIntent.slots.PatientID;

        let responseMessage = '';

        switch (billQueryType) {
            case 'OutstandingBalance':
                responseMessage = `The outstanding balance for patient ID ${patientID} is $250.`;
                break;
            case 'PaymentDueDate':
                responseMessage = `The next payment due date for patient ID ${patientID} is on August 31, 2024.`;
                break;
            case 'RecentPayments':
                responseMessage = `The most recent payment for patient ID ${patientID} was $100 on July 15, 2024.`;
                break;
            default:
                responseMessage = `I'm sorry, I don't have information on that topic. Please try asking about Outstanding Balance, Payment Due Date, or Recent Payments.`;
        }

        return {
            dialogAction: {
                type: 'Close',
                fulfillmentState: 'Fulfilled',
                message: {
                    contentType: 'PlainText',
                    content: responseMessage,
                },
            },
        };
    }

    // Handle other intents
    return {
        dialogAction: {
            type: 'Close',
            fulfillmentState: 'Failed',
            message: {
                contentType: 'PlainText',
                content: 'Sorry, I could not process your request.',
            },
        },
    };
};

};
