# Human Approval Mechanism

## Purpose

The Human Review stage ensures that the AI does not make the final recruitment decision.

The AI only provides an assessment and recommendation. An authorized human reviewer makes the final decision.

## Approval Flow

```text
AI Analysis
     ↓
AI Recommendation
     ↓
Human Review
   ↙       ↘
Approve    Reject
   ↓         ↓
Shortlist   Reject

# Human Review & Error Handling

## Information Shown to Reviewer

The human reviewer receives:

- Candidate name
- Candidate email
- Degree
- Skills
- Experience
- Availability
- AI score
- AI recommendation
- AI reason

## Human Decision

### Approve

If the reviewer approves the candidate:

```
final_decision = Shortlisted
status = Human Approved
```

The workflow then sends the appropriate candidate email and updates the candidate record.

### Reject

If the reviewer rejects the candidate:

```
final_decision = Rejected
status = Human Rejected
```

The workflow sends a rejection email and updates the candidate record.

## Important Security Principle

The AI recommendation is never treated as the final hiring decision.

Even if the AI recommends "Shortlist," the workflow waits for an authorized human to approve the candidate. The human reviewer therefore remains responsible for the final selection decision.

---

# `error-handling.md`

## Error Handling

### Purpose

The recruitment automation includes error handling so that failures do not silently break the workflow.

### Error Workflow

A separate n8n error workflow is used:

```text
Error Trigger
      ↓
Log Error
      ↓
Notify Administrator
```

The Error Trigger workflow is configured separately from the main recruitment workflow.

### Errors Considered

The system accounts for possible failures including:

- Missing candidate information
- Invalid candidate data
- Invalid AI output
- Invalid JSON
- Invalid AI score
- Groq/API failures
- Google Sheets failures
- Email failures
- Workflow execution failures

### Error Logging

The error handler records relevant debugging information such as:

- Workflow name
- Execution ID
- Error message
- Timestamp

Sensitive information should not be included in error logs. The system must not log:

- API keys
- Passwords
- Webhook secrets
- Unnecessary candidate PII
- Credentials

### Administrator Notification

When a workflow failure occurs, the error workflow sends an alert to the administrator. The notification contains the error information required for debugging without exposing sensitive credentials or unnecessary candidate information.

### Graceful Failure

If candidate information is missing or the AI produces invalid structured output, the workflow should prevent invalid data from continuing through the recruitment process. The candidate should not automatically be shortlisted or rejected because of a technical failure.

### Human Control

Technical errors must never result in an automatic hiring decision. Final recruitment decisions remain under human control.
