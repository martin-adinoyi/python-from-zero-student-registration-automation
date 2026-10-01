# Architecture

## Overview

The workflow is a registration pipeline with three major concerns:

1. **Input validation**
2. **Registration state management**
3. **User communication**

```text
                   ┌─────────────────────┐
                   │   Registration Form │
                   └──────────┬──────────┘
                              │
                              ▼
                   ┌─────────────────────┐
                   │ Email/Phone         │
                   │ Validation          │
                   └──────────┬──────────┘
                              │
                   ┌──────────┴──────────┐
                   │                     │
                Invalid                 Valid
                   │                     │
                   ▼                     ▼
            ┌─────────────┐      ┌─────────────┐
            │ Error Page  │      │ Configuration│
            └─────────────┘      └──────┬──────┘
                                        │
                                        ▼
                              ┌──────────────────┐
                              │ Google Sheets    │
                              │ Duplicate Check  │
                              └────────┬─────────┘
                                       │
                              ┌────────┴────────┐
                              │                 │
                           Exists           New user
                              │                 │
                              ▼                 ▼
                    ┌────────────────┐  ┌──────────────┐
                    │ Already        │  │ Save to      │
                    │ Registered     │  │ Google Sheets│
                    └────────────────┘  └──────┬───────┘
                                                │
                                                ▼
                                      ┌──────────────────┐
                                      │ Confirmation     │
                                      │ Email            │
                                      └────────┬─────────┘
                                               │
                                               ▼
                                      ┌──────────────────┐
                                      │ WhatsApp Group   │
                                      │ Handoff Page     │
                                      └──────────────────┘
```

## Why duplicate detection happens before saving

Email is used as the registration identifier in the original implementation.

The workflow searches the student sheet for the submitted email before appending a new row.

This prevents the same email from being registered repeatedly.

## Data flow

```text
Student
  │
  ▼
n8n Form
  │
  ├── Full Name
  ├── Email
  ├── WhatsApp Number
  ├── Location
  ├── Programming Experience
  ├── Device Available
  └── Why do you want to join?
  │
  ▼
Validation
  │
  ▼
Duplicate Check
  │
  ▼
Google Sheets
  │
  ▼
Gmail
  │
  ▼
WhatsApp
```

## External services

| Service | Role |
|---|---|
| n8n | Workflow orchestration and form handling |
| Google Sheets | Registration database |
| Gmail | Confirmation email |
| WhatsApp | Community/session destination |

## Production considerations

This is intentionally a lightweight registration system. For a larger production application, consider:

- Normalizing email addresses before duplicate checks.
- Normalizing phone numbers to E.164 format.
- Adding a unique registration ID.
- Adding explicit consent/privacy language.
- Adding error handling for Google Sheets and Gmail failures.
- Moving configuration into a dedicated configuration layer.
- Adding monitoring/alerting.
