# Candidate Screening Automation

## Problem Statement

Recruiters often spend significant time manually reviewing internship applications, validating candidate information, categorizing applicants based on eligibility, updating records, and sending emails. This manual process is repetitive, time-consuming, and prone to human error.

---

## Objective

The objective of this workflow is to automate the internship candidate screening process using n8n. The workflow reads candidate information from Google Sheets, validates the required fields, applies predefined screening rules, updates the screening status in Google Sheets, and sends personalized email notifications to each candidate.

---

## Workflow Architecture

```
Manual Trigger
      │
      ▼
Google Sheets (Read Rows)
      │
      ▼
IF (Validate Required Fields)
      │
      ▼
Code (Apply Screening Rules)
      │
      ▼
Switch (Categorize Candidate)
      ├────────► Shortlisted
      │              │
      │              ▼
      │      Google Sheets (Update Row)
      │              │
      │              ▼
      │         Send Email
      │
      ├────────► Needs Review
      │              │
      │              ▼
      │      Google Sheets (Update Row)
      │              │
      │              ▼
      │         Send Email
      │
      └────────► Not Eligible
                     │
                     ▼
             Google Sheets (Update Row)
                     │
                     ▼
                Send Email
```

---

## Technologies Used

- n8n
- Google Sheets
- Gmail SMTP
- JavaScript
- JSON
- Google Workspace

---

## Nodes Used

- Manual Trigger
- Google Sheets (Read Rows)
- IF
- Code (JavaScript)
- Switch
- Google Sheets (Update Row)
- Send Email

---

## Setup Instructions

1. Import the workflow into n8n.
2. Create a Google Sheet containing the following columns:

- Name
- Email
- Degree
- Skills
- Experience
- Availability
- Status

3. Connect your Google Sheets account.
4. Configure SMTP or Gmail credentials.
5. Select the spreadsheet inside the Google Sheets nodes.
6. Execute the workflow.

---

## Credentials Required

Only credential names are listed below.

- Google Sheets OAuth2
- Gmail OAuth2 or SMTP

**Do not store passwords or API keys inside the workflow or repository.**

---

## Workflow Explanation

1. Read candidate records from Google Sheets.
2. Validate that all required fields are present.
3. Apply screening rules using JavaScript.
4. Categorize each candidate into:
   - Shortlisted
   - Needs Review
   - Not Eligible
5. Update the Status column in Google Sheets.
6. Send a personalized email to each candidate according to the screening result.

---

## Screening Rules

### Shortlisted

- Experience ≥ 2 years
- Availability = Yes
- Skills contain Python or n8n

### Needs Review

- Experience ≥ 1 year
- Does not satisfy all Shortlisted conditions

### Not Eligible

- Experience = 0 years
- OR Availability = No

---

## Test Cases

| Candidate | Expected Result |
|------------|-----------------|
| Experience = 3, Availability = Yes, Skills = Python | Shortlisted |
| Experience = 1, Availability = Yes | Needs Review |
| Experience = 0 | Not Eligible |
| Availability = No | Not Eligible |

---

## Error Handling

- Missing required fields are filtered using the IF node.
- Invalid records are not processed further.
- Google Sheets authentication errors require reconnecting credentials.
- Email sending errors are reported by the Send Email node.

---

## Known Limitations

- Screening rules are hardcoded inside the Code node.
- Only one spreadsheet structure is supported.
- Email templates are static.
- Does not check duplicate applications.

---

## Future Improvements

- Build a web application for candidate submission.
- Add resume parsing.
- Integrate AI-based resume scoring.
- Support PDF resume uploads.
- Store candidate records in a database.
- Notify recruiters through Slack or Microsoft Teams.
- Generate interview schedules automatically.

---

## Repository Structure

```
Candidate-Screening-Automation/
│
├── workflow/
│   └── workflow.json
│
├── screenshots/
│
├── sample-data/
│
└── README.md
```

---

## Author

**Muhammad Bin Waseem**

AI Automation Internship

Petalnex Pvt. Ltd.
