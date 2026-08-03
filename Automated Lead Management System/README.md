# Automated Lead Management System

## Project Overview

The Automated Lead Management System is an end-to-end workflow built in n8n that automatically receives new business leads through a Webhook, validates the submitted information, calculates a lead score, routes leads based on their priority, stores lead information, notifies the sales team, and sends an acknowledgment email to the customer.

This workflow demonstrates the integration of multiple systems into a single automation pipeline using Webhooks, Google Sheets, Email, and JavaScript.

---

## Problem Statement

Manually reviewing and categorizing incoming leads is time-consuming and may delay responses to potential customers. Businesses need an automated solution that instantly validates, scores, categorizes, stores, and notifies the appropriate team.

---

## Objective

- Receive new leads through a Webhook.
- Validate all required fields.
- Automatically calculate a lead score.
- Categorize leads into High, Medium, and Low Priority.
- Notify the sales team for urgent leads.
- Save leads into Google Sheets.
- Send an acknowledgment email to every customer.

---

## Workflow Architecture

```
Webhook
   ↓
IF (Validate Input)
   ↓
Code (Lead Processing + Lead Scoring)
   ↓
Switch (Priority Routing)
      ├── High Priority
      │       └── Send Sales Notification Email
      │
      ├── Medium Priority
      │       └── Save to Google Sheets
      │
      └── Low Priority
              └── Save to Follow-up Sheet

(All branches)
        ↓
Send Acknowledgment Email
```

---

## Technologies Used

- n8n
- Webhooks
- JavaScript (Code Node)
- Google Sheets
- Gmail SMTP
- Postman
- JSON

---

## Nodes Used

- Webhook
- IF
- Code
- Switch
- Google Sheets
- Send Email

---

## Setup Instructions

1. Create a Webhook node.
2. Configure the IF node to validate incoming fields.
3. Use a Code node to calculate lead score and priority.
4. Add a Switch node to route leads.
5. Connect High Priority to Sales Email.
6. Connect Medium Priority to Google Sheets.
7. Connect Low Priority to Follow-up Sheet.
8. Add acknowledgment email nodes.
9. Test using Postman.

---

## Credentials Required

- Google Sheets Credentials
- Gmail SMTP Credentials

**Do not include passwords or API keys.**

---

## Workflow Explanation

1. The Webhook receives lead information.
2. The IF node validates required fields.
3. The Code node calculates a lead score based on the customer's budget.
4. The Switch node routes the lead according to priority.
5. High-priority leads immediately notify the sales team.
6. Medium-priority leads are stored in Google Sheets.
7. Low-priority leads are saved for future follow-up.
8. Every customer receives an automatic acknowledgment email.

---

## Sample API Request

```json
{
  "name": "Hatim Warrior",
  "email": "hatim@gmail.com",
  "company": "Petalnex",
  "service": "AI Automation",
  "budget": 12000
}
```

---

## Test Cases

### Test Case 1

Budget: 12000

Expected Result:

- Lead Score = 100
- Priority = High
- Sales Email Sent

---

### Test Case 2

Budget: 7000

Expected Result:

- Lead Score = 70
- Priority = Medium
- Saved to Google Sheets

---

### Test Case 3

Budget: 2500

Expected Result:

- Lead Score = 40
- Priority = Low
- Saved to Follow-up Sheet

---

## Error Handling

- Missing required fields are rejected by the IF node.
- Invalid requests are not processed further.
- Failed email or Google Sheets operations can be retried from n8n executions.

---

## Known Limitations

- Lead scoring is based only on budget.
- No duplicate lead detection.
- No CRM integration.
- No authentication for Webhook endpoint.

---

## Future Improvements

- CRM Integration (HubSpot/Salesforce)
- Duplicate Detection
- AI-Based Lead Scoring
- Slack Notifications
- Database Storage
- Dashboard & Analytics

- DEMO :- https://drive.google.com/file/d/1Ac9RbdfgoltQn6keV2mNi1-4M6hZxZ8i/view?usp=sharing
