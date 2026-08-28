# AI-Assisted Recruitment Automation

## Project Name

AI-Assisted Recruitment Automation

## Problem Statement

Recruitment teams receive many candidate applications and need to process candidate information efficiently.

Manually extracting candidate information, reviewing skills, assessing qualifications, and communicating with candidates can be time-consuming.

This project demonstrates an AI-assisted recruitment automation that analyzes candidate applications while keeping the final selection decision under human control.

---

## Objective

The objective is to build a secure and reliable recruitment workflow where:

- Candidate applications are collected automatically.
- Candidate information is stored.
- AI extracts skills and evaluates the application.
- AI generates a score and advisory recommendation.
- A human reviewer makes the final shortlist/reject decision.
- Candidates receive an appropriate email.
- The candidate record is updated.
- Errors are captured and reported.

The AI acts as a decision-support tool and does not make the final hiring decision.

---

## Workflow Architecture

```text
Candidate Application
        ↓
      Webhook
        ↓
Store Candidate Data
        ↓
Extract Candidate Information
        ↓
AI Analysis + Skills Extraction
        ↓
Structured Output Validation
        ↓
Rule-Based Assessment
        ↓
    Human Review
       ↙     ↘
  Approve    Reject
      ↓        ↓
 Shortlist   Reject
       ↘      ↙
   Candidate Email
          ↓
 Update Candidate Record
```

### Error Workflow

```text
Error Trigger
      ↓
  Log Error
      ↓
Notify Administrator
```

---

## Technologies Used

- n8n
- Groq API
- Llama
- Google Sheets
- Webhooks
- Structured Output / JSON Schema
- IF Node
- Code Node
- Wait / Human Approval
- Gmail / SMTP
- Error Trigger
- JavaScript expressions

---

## Nodes Used

### Main Workflow

- Candidate Application — Webhook
- Validate Application
- Store Candidate Data
- Extract Candidate Information
- AI Candidate Analysis
- Structured Output Parser
- Rule-Based Assessment
- Human Review
- Shortlist / Reject
- Candidate Email
- Update Candidate Record

### Error Workflow

- Error Trigger
- Log Error
- Notify Administrator

---

## Candidate Input

The webhook accepts candidate information in JSON format:

```json
{
  "name": "Ali Khan",
  "email": "ali@example.com",
  "degree": "BS Computer Science",
  "skills": "Python, C++, SQL",
  "experience": "1 year software development internship",
  "availability": "Immediate"
}
```

## AI Output

The AI produces structured output:

```json
{
  "candidate": "Ali Khan",
  "skills": ["Python", "C++", "SQL"],
  "score": 82,
  "recommendation": "Shortlist",
  "reason": "The candidate has relevant technical skills and internship experience."
}
```

The AI score and recommendation are advisory only.

---

## Human Approval

The workflow pauses at the Human Review stage.

The reviewer can choose:

### Approve

The candidate is shortlisted.

```
final_decision = Shortlisted
status = Human Approved
```

### Reject

The candidate is rejected.

```
final_decision = Rejected
status = Human Rejected
```

The AI cannot bypass this approval stage.

---

## Google Sheets

Candidate information is stored in a Google Sheet.

The sheet contains:

- candidate_id
- name
- email
- degree
- skills
- experience
- availability
- ai_score
- ai_recommendation
- ai_reason
- final_decision
- status
- created_at
- reviewed_at

The AI analysis and final human decision are stored separately so that the final decision can be distinguished from the AI recommendation.

---

## Setup Instructions

### 1. Create Google Sheet

Create a Google Sheet named:

```
Recruitment Candidates
```

Add the required columns:

- candidate_id
- name
- email
- degree
- skills
- experience
- availability
- ai_score
- ai_recommendation
- ai_reason
- final_decision
- status
- created_at
- reviewed_at

### 2. Configure n8n

Import the workflow JSON into n8n.

Configure the required credentials in the appropriate nodes.

### 3. Configure Groq

Connect the Groq credential to the AI model node.

Use the configured Llama model.

### 4. Configure Google Sheets

Connect the Google Sheets credential and select the recruitment spreadsheet.

### 5. Configure Email

Connect the Gmail/SMTP credential used to send candidate and administrator emails.

### 6. Configure Error Workflow

Open the recruitment workflow settings and assign the separate:

```
Recruitment Automation Error Handler
```

as its error workflow.

### 7. Test the Webhook

Send a POST request containing candidate information to the webhook.

---

## Credentials Required

Only credential names are listed here.

- Groq API Credential
- Google Sheets OAuth2 Credential
- Gmail / SMTP Credential

No API keys, passwords, webhook secrets, or credential values are included in this repository.

---

## Workflow Explanation

A candidate submits an application through the webhook.

The application information is stored and normalized.

The AI then analyzes the candidate's provided qualifications and extracts relevant skills.

The AI produces:

- Candidate
- Skills
- Score
- Recommendation
- Reason

The result is then checked using deterministic logic.

The workflow does not automatically make a hiring decision.

Instead, the workflow pauses at the Human Review stage.

An authorized human reviewer examines the candidate information and AI recommendation.

The reviewer then chooses whether to shortlist or reject the candidate.

After the human decision, the workflow sends the appropriate candidate email and updates the candidate record.

If a technical failure occurs, the separate error workflow captures the error and notifies the administrator.

---

## Error Handling

The system handles potential failures including:

- Missing candidate fields
- Invalid AI output
- Invalid JSON
- API failures
- Google Sheets failures
- Email failures
- Workflow execution errors

The separate Error Trigger workflow logs the failure and notifies the administrator.

Sensitive information such as API keys, passwords, webhook secrets, and unnecessary candidate PII is not included in error notifications.

---

## Security

The workflow follows basic security practices:

- Credentials are stored using n8n credential management.
- API keys are not hard-coded.
- Passwords are not stored in workflow logic.
- Secrets are not committed to GitHub.
- Candidate PII is minimized.
- Sensitive credentials are not included in screenshots.
- AI does not make the final hiring decision.
- Human approval is required before the final recruitment action.

---

## Test Cases

The workflow should be tested using multiple candidate profiles.

| # | Test Case | Expected Result |
|---|-----------|------------------|
| 1 | Strong candidate | AI gives high score → Human Review |
| 2 | Weak candidate | AI gives low score → Human Review |
| 3 | Candidate with missing information | Validation/error path |
| 4 | Strong skills but little experience | AI evaluates provided information → Human Review |
| 5 | Mixed qualifications | AI provides assessment → Human Review |
| 6 | Human approves candidate | Candidate shortlisted and email sent |
| 7 | Human rejects candidate | Candidate rejected and email sent |
| 8 | Invalid AI output | Structured output validation/error handling |
| 9 | API failure | Error workflow triggered |
| 10 | Google Sheets failure | Error workflow triggered |

---

## AI Decision vs Human Decision

The system deliberately separates the AI recommendation from the final human decision.

```text
AI Recommendation
       ↓
  Human Review
       ↓
 Final Decision
```

This prevents the AI from independently making consequential recruitment decisions.

---

## Known Limitations

- AI recommendations may contain errors.
- AI scoring depends on the quality of the provided candidate information.
- The system does not replace professional recruitment judgment.
- External APIs may experience downtime or rate limits.
- Email and Google Sheets services may fail.
- Candidate information requires appropriate privacy controls.
- The current workflow is intended as an internship demonstration rather than a complete production recruitment platform.

---

## Future Improvements

- Add authentication to the candidate webhook.
- Add stronger input validation.
- Add duplicate candidate detection.
- Add a dedicated recruitment database.
- Add role-specific evaluation criteria.
- Add recruiter dashboards.
- Add audit trails for human decisions.
- Add retry logic for temporary API failures.
- Add confidence scoring.
- Add more advanced monitoring and alerting.
- Add role-based access control for reviewers.
- Add multilingual candidate processing.
