# AI Customer Support Triage System

## Project Name

AI Customer Support Triage System

## Problem Statement

Customer support teams receive a large number of messages covering different issues such as billing problems, technical difficulties, account issues, product questions, and complaints.

Manually reviewing and routing every message can be time-consuming and can delay responses to urgent customers.

This project uses an AI-powered n8n workflow to automatically analyze incoming customer support messages, classify them, determine their priority and sentiment, assign the appropriate department, generate a summary and suggested response, store the ticket, and send appropriate notifications.

## Objective

The objective of this project is to build a production-style AI customer support triage workflow using n8n.

The system automatically:

- Receives customer support messages through a Webhook.
- Sends the message to an LLM for analysis.
- Produces predictable structured JSON output.
- Determines the category, priority, sentiment, and department.
- Generates a summary and suggested response.
- Routes the ticket according to its category.
- Stores the support ticket.
- Sends an acknowledgment email.
- Sends an urgent alert when the ticket is classified as High priority.

## Workflow Architecture

```text
Customer Message
       ↓
    Webhook
       ↓
  AI Analysis
       ↓
Structured Output Parser
       ↓
    Validation
       ↓
 Switch by Category
       ├── Billing
       ├── Technical Support
       ├── Account
       ├── Product
       ├── Complaint
       └── General
              ↓
        Store Ticket
              ↓
      Priority Check
         ├── High → Urgent Alert
         └── Other
              ↓
    Acknowledgment Email
