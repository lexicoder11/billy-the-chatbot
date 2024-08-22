# Billy the Chatbot

## Introduction
### Project Overview
This project is a medical customer service chatbot designed to assist patients with medical bill inquiries and payments. The chatbot leverages AWS services for natural language understanding, serverless functions, and data storage.

### Technology Stack
- **Amazon Lex**: For natural language understanding and chatbot interactions.
- **Amazon DynamoDB** : For persistent data storage.
- **AWS IAM**: For managing permissions and roles.
- **Amazon CloudWatch**: For logging and monitoring.

## Architecture
### System Architecture Diagram
![Architecture Diagram](link_to_diagram.png)

### Component Descriptions
- **Amazon Lex Bot**: Manages user interactions and understands user intent.
- **Amazon DynamoDB**: Stores user data.

## Setup and Configuration
### Prerequisites
- AWS account
- Basic knowledge of AWS services
- IAM roles and policies setup

### AWS Setup
#### IAM Roles and Permissions
1. Create IAM roles for Lex with the necessary permissions.
2. Attach policies for DynamoDB access.

### Creating the Lex Bot
#### Bot Configuration
1. Go to Amazon Lex console.
2. Create a new bot with the desired name and language.
3. Configure basic settings.

#### Defining Intents and Sample Utterances
1. Create intents titled `Bill`,`Payments`.
2. Add sample utterances for the intent.

#### Adding and Configuring Slots
1. Define slots `BillQuestion`, `PaymentOptions`,`Claims`, and `GeneralBillQuestions`
2. Configure prompts for slot elicitation.

