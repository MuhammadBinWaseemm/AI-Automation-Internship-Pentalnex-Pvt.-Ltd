# Lead Processing Automation

## Problem Statement

Sales teams often receive lead information from multiple sources. Manually validating, enriching, storing, and notifying team members is repetitive and time-consuming.

---

## Objective

Build an automated workflow that:

- Receives lead information via a Webhook
- Validates required fields
- Enriches the lead using an external API
- Stores the processed lead in Google Sheets
- Sends a notification email automatically

---

## Workflow Architecture

Webhook
↓
IF (Validate Lead)
↓
HTTP Request (API Lookup / Enrich)
↓
Google Sheets (Save Lead)
↓
Send Email (Notify Sales Team)

---

## Technologies Used

- n8n
- Webhook
- HTTP Request
- REST API
- Google Sheets
- Gmail / SMTP
- JSON
- Postman

---

## Nodes Used

1. Webhook
2. IF
3. HTTP Request
4. Google Sheets
5. Send Email

---

## Setup Instructions

1. Create a Webhook node.
2. Send sample lead data using Postman.
3. Validate required fields using an IF node.
4. Configure an HTTP Request node to enrich the lead.
5. Connect Google Sheets and append the processed lead.
6. Configure the Email node using Gmail SMTP.
7. Execute the workflow and verify each step.

---

## Credentials Required

- Google Sheets Credentials
- Gmail SMTP Credentials
- External API (if authentication is required)

Do not include passwords or API keys.

---

## Workflow Explanation

The workflow begins when a webhook receives lead information.

The IF node verifies that mandatory fields are present.

The HTTP Request node enriches the lead using an external API.

The enriched lead is stored in Google Sheets.

Finally, an email notification is sent confirming that the lead has been processed.

---

## Test Cases

### Valid Lead

Input:
- Name
- Email
- Company

Expected:
- Lead saved to Google Sheets
- Notification email sent

---

### Invalid Lead

Missing required field.

Expected:
- Validation fails.
- Workflow stops.
- No record is stored.

---

## Error Handling

- Validate required fields before processing.
- Handle invalid API responses.
- Prevent incomplete records from being stored.

---

## Known Limitations

- Uses sample lead data.
- Depends on internet connectivity.
- API enrichment depends on the availability of the external service.

---

## Future Improvements

- CRM integration (HubSpot/Salesforce)
- Duplicate lead detection
- Slack or Microsoft Teams notifications
- Automatic lead scoring using AI
- Error logging and retry mechanism
