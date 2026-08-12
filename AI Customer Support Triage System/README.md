# AI Customer Support Triage System

An AI-powered customer support triage system built with **n8n**, **Groq**, and **Llama 3.3 70B Versatile**.

The workflow automatically analyzes incoming customer support messages, classifies them, determines their priority and sentiment, assigns the appropriate department, generates a summary and suggested response, stores the ticket, sends an acknowledgment email, and alerts the support team when a ticket is classified as high priority.

---

## Table of Contents

* [Overview](#overview)
* [Objectives](#objectives)
* [Workflow Architecture](#workflow-architecture)
* [Technologies Used](#technologies-used)
* [Workflow Nodes](#workflow-nodes)
* [AI Classification](#ai-classification)
* [Structured JSON Output](#structured-json-output)
* [Project Structure](#project-structure)
* [Setup Instructions](#setup-instructions)
* [Credentials Required](#credentials-required)
* [Testing](#testing)
* [Error Handling](#error-handling)
* [Known Limitations](#known-limitations)
* [Future Improvements](#future-improvements)
* [Deliverables](#deliverables)
* [Project Status](#project-status)

---

## Overview

Customer support teams receive a large volume of messages covering billing issues, technical difficulties, account problems, product questions, complaints, and general inquiries.

Manually reviewing and routing every message can be time-consuming and may delay responses to urgent customers.

This project uses an **AI-powered n8n workflow** to automate the initial customer support triage process.

The system receives a customer message through a webhook and uses an LLM to analyze the request. The AI generates structured information that is then validated and used to route, store, and prioritize the support ticket.

---

## Objectives

The system automatically:

* Receives customer support messages through a Webhook.
* Sends the message to an LLM for analysis.
* Produces predictable structured JSON output.
* Determines the ticket category.
* Determines ticket priority.
* Detects customer sentiment.
* Assigns the appropriate department.
* Generates a concise ticket summary.
* Generates a suggested customer response.
* Routes the ticket according to its category.
* Stores the processed ticket.
* Sends an acknowledgment email.
* Sends an urgent alert for high-priority tickets.

---

## Workflow Architecture

```text
Customer Message
       |
       v
    Webhook
       |
       v
  AI Analysis
       |
       v
Structured Output Parser
       |
       v
    Validation
       |
       v
 Switch by Category
       |
       +---- Billing
       |
       +---- Technical Support
       |
       +---- Account
       |
       +---- Product
       |
       +---- Complaint
       |
       +---- General
       |
       v
   Store Ticket
       |
       v
  Priority Check
       |
       +---- High ------> Urgent Alert
       |
       +---- Other
              |
              v
     Acknowledgment Email
```

---

## Technologies Used

| Technology                   | Purpose                                    |
| ---------------------------- | ------------------------------------------ |
| **n8n**                      | Workflow automation and orchestration      |
| **Groq API**                 | LLM inference                              |
| **Llama 3.3 70B Versatile**  | Customer message analysis                  |
| **Webhook**                  | Receives incoming support requests         |
| **Structured Output Parser** | Enforces predictable JSON output           |
| **JSON Schema**              | Defines the expected AI response structure |
| **Switch Node**              | Routes tickets by category                 |
| **IF Node**                  | Checks ticket priority                     |
| **Google Sheets**            | Stores processed tickets                   |
| **Gmail / SMTP**             | Sends customer acknowledgment emails       |
| **Postman**                  | Tests the webhook                          |
| **JavaScript Expressions**   | Data processing and workflow logic         |

---

## Workflow Nodes

### 1. Webhook

The Webhook node receives customer support information through an HTTP `POST` request.

Example input:

```json
{
  "name": "Ali Khan",
  "email": "ali@example.com",
  "message": "I have been charged twice for my subscription."
}
```

---

### 2. AI Analysis

The AI Analysis node sends the customer's message to the Groq Chat Model.

The model analyzes the message and determines:

* Category
* Priority
* Sentiment
* Department
* Summary
* Suggested response

The model follows the structured prompt defined in [`prompt.txt`](prompt.txt).

---

### 3. Structured Output Parser

The Structured Output Parser ensures that the AI returns data in a predictable JSON format instead of unrestricted text.

This makes the AI output easier to validate and process in later workflow nodes.

---

### 4. Validation

The generated AI output is validated to ensure:

* Required fields are present.
* Values use the permitted categories.
* The output follows the expected structure.

Invalid output can be routed to a fallback path instead of being processed as a valid ticket.

---

### 5. Switch

The Switch node routes tickets according to their category.

Supported categories:

* Billing
* Technical Support
* Account
* Product
* Complaint
* General

---

### 6. Google Sheets

The processed ticket is stored in Google Sheets for tracking and further processing.

Stored information can include:

* Customer name
* Customer email
* Original message
* Category
* Priority
* Sentiment
* Department
* Summary
* Suggested response

---

### 7. Acknowledgment Email

After the ticket has been processed, an acknowledgment email is sent to the customer.

The email confirms that the support request has been received and provides the customer with an appropriate response based on the AI-generated analysis.

---

### 8. IF — Priority Check

The IF node checks the ticket priority.

If:

```text
priority = High
```

the workflow sends an urgent alert to the support team.

Medium- and Low-priority tickets continue through the normal workflow without triggering the urgent alert.

---

## AI Classification

### Category

Allowed values:

* `Billing`
* `Technical Support`
* `Account`
* `Product`
* `Complaint`
* `General`

### Priority

Allowed values:

* `High`
* `Medium`
* `Low`

### Sentiment

Allowed values:

* `Positive`
* `Neutral`
* `Negative`

### Department

Allowed values:

* `Billing`
* `Technical Support`
* `Account Management`
* `Product Support`
* `Customer Service`
* `General`

---

## Structured JSON Output

The AI is required to return the following structure:

```json
{
  "category": "Billing",
  "priority": "High",
  "sentiment": "Negative",
  "department": "Billing",
  "summary": "Customer reports being charged twice for a subscription.",
  "suggested_response": "We apologize for the duplicate charge and will investigate the issue."
}
```

The complete JSON Schema is available in [`structured-schema.json`](structured-schema.json).

---

## AI Prompt

The AI uses a structured prompt that defines:

* Its role
* Context
* Classification task
* Business rules
* Allowed values
* Expected output format

The complete prompt is stored separately in:

[`prompt.txt`](prompt.txt)

---

## Project Structure

```text
AI-Customer-Support-Triage-System/
│
├── workflow/
│   └── workflow.json
│
├── screenshots/
│   ├── webhook.png
│   ├── ai-analysis.png
│   ├── structured-output.png
│   ├── routing.png
│   ├── stored-ticket.png
│   └── urgent-alert.png
│
├── sample-data/
│   └── test-cases.json
│
├── prompt.txt
├── structured-schema.json
└── README.md
```

---

## Setup Instructions

### 1. Open n8n

Open your n8n instance and import the workflow file:

```text
workflow/workflow.json
```

---

### 2. Configure the AI Model

Configure the **Groq Chat Model** used by the AI Analysis node.

Select the appropriate Groq credential and configure the model:

```text
Llama 3.3 70B Versatile
```

---

### 3. Configure the Webhook

The Webhook node should use:

```text
HTTP Method: POST
```

During development and testing, use the **Webhook Test URL** provided by n8n.

---

### 4. Send Test Data

Use Postman or another HTTP client to send a `POST` request to the n8n Webhook Test URL.

Header:

```text
Content-Type: application/json
```

Example request body:

```json
{
  "name": "Ali Khan",
  "email": "ali@example.com",
  "message": "I have been charged twice for my subscription and need this fixed immediately."
}
```

---

### 5. Test the Workflow

Verify that:

* The Webhook receives the request.
* The AI analyzes the customer message.
* Structured JSON is generated.
* The output passes validation.
* The ticket is routed according to its category.
* The ticket is stored.
* The customer receives an acknowledgment email.
* High-priority tickets trigger an urgent alert.

---

## Credentials Required

Only credential names are required in the workflow repository.

### Required Credentials

* Groq API Credential
* Google Sheets OAuth2 Credential
* Gmail / SMTP Credential

> **Security:** Never commit API keys, passwords, OAuth secrets, tokens, or other credentials to the repository.

---

## Testing

The workflow should be tested using customer messages from different categories, priorities, and sentiment levels.

### Test Cases

|  # | Customer Message                                     | Expected Category | Expected Priority |
| -: | ---------------------------------------------------- | ----------------- | ----------------- |
|  1 | I was charged twice for my subscription.             | Billing           | High              |
|  2 | The application crashes whenever I upload a file.    | Technical Support | Medium            |
|  3 | I cannot log into my account.                        | Account           | High              |
|  4 | How can I change my product plan?                    | Product           | Low               |
|  5 | I am extremely unhappy with your service.            | Complaint         | High              |
|  6 | What are your support hours?                         | General           | Low               |
|  7 | My payment failed while purchasing the service.      | Billing           | High              |
|  8 | The application is running slowly.                   | Technical Support | Medium            |
|  9 | I would like information about one of your products. | Product           | Low               |
| 10 | I need help understanding my account settings.       | Account           | Medium            |


## Error Handling

The workflow uses structured output validation to reduce the possibility of malformed AI responses.

If the AI output:

* Does not contain required fields.
* Uses an invalid category.
* Uses an invalid priority.
* Uses an invalid sentiment.
* Uses an invalid department.
* Does not match the expected JSON structure.

the workflow can route the item to a fallback path instead of continuing with invalid data.

The priority check also ensures that only tickets classified as `High` trigger urgent alerts.

---

## Known Limitations

* AI classification can still produce incorrect predictions.
* Classification quality depends on the selected model and prompt.
* The workflow requires valid API and service credentials.
* External services may experience rate limits or downtime.
* AI-generated suggested responses may require human review.
* The current implementation is intended primarily as an internship/project demonstration.
* Additional security, monitoring, authentication, and reliability measures would be required before production deployment.
* Sensitive or complex customer issues should be reviewed by a human support agent.

---

## Future Improvements

Potential improvements include:

* Add webhook authentication and security.
* Add retry logic for temporary API failures.
* Add AI confidence scoring.
* Add human approval for sensitive or high-risk tickets.
* Add duplicate-ticket detection.
* Replace Google Sheets with a dedicated ticket database.
* Add support analytics and dashboards.
* Add automatic SLA-based escalation.
* Add multilingual customer support.
* Evaluate multiple LLM providers.
* Test classification accuracy on a larger labeled dataset.
* Add workflow monitoring and error notifications.
* Add rate limiting and abuse protection.
* Add audit logging.

---
