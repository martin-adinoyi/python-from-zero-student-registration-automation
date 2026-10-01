# n8n Student Registration Automation

A reusable **n8n workflow template for automating student registration** for an online program, course, workshop, or community.

This repository contains the workflow originally built for **Python From Zero — Free Beginner Program**, but the automation is structured so it can be adapted to other registration use cases.

## What it does

The workflow automates the registration journey:

```text
Registration Form
       ↓
Email + Phone Validation
       ↓
Duplicate Registration Check
       ↓
      ┌───────────────┐
      │ Already exists│
      └───────┬───────┘
              ↓
      Show existing-registration page

New registration
       ↓
Save to Google Sheets
       ↓
Send confirmation email
       ↓
Show WhatsApp group page
```

### Main capabilities

- Custom n8n registration form
- Required registration fields
- Email format validation
- WhatsApp/phone number validation
- Duplicate registration detection using email
- Google Sheets storage
- Automated confirmation email
- WhatsApp community/group handoff
- Custom HTML/CSS response pages
- Separate handling for successful and duplicate registrations

The original form collects full name, email, WhatsApp number, location, programming experience, device availability, and reason for joining.

## Repository structure

```text
n8n-student-registration-automation/
│
├── workflow/
│   └── python-from-zero-student-registration.json
│
├── docs/
│   ├── architecture.md
│   └── setup.md
│
├── assets/
│   └── .gitkeep
│
├── .gitignore
├── LICENSE
└── README.md
```

## Requirements

- [n8n](https://n8n.io/)
- Google account
- Google Sheets
- Gmail
- WhatsApp group/community invite link
- An n8n instance capable of running Form, Google Sheets, Gmail, Code, IF, Set, and Respond-to-Form nodes

## Installation

1. Download the workflow JSON from `workflow/`.
2. Open your n8n instance.
3. Import the JSON workflow.
4. Configure the Google Sheets credential.
5. Configure the Gmail credential.
6. Create/configure the destination Google Sheet.
7. Replace the placeholder WhatsApp invite URL.
8. Replace the placeholder n8n form URL used by the validation-error page.
9. Review the form fields and program information.
10. Test the workflow before activating it.

See [`docs/setup.md`](docs/setup.md) for the full setup procedure.

## Google Sheets structure

The workflow expects these columns:

| Column | Purpose |
|---|---|
| REGISTRATION DATE | Date/time of submission |
| FULL NAME | Student's name |
| EMAIL | Student's email |
| WHATSAPP NUMBER | Student's WhatsApp number |
| LOCATION | Student's location |
| PROGRAMMING EXPERIENCE | Experience level |
| DEVICE AVAILABLE | Available device |
| WHY DO YOU WANT TO JOIN | Registration motivation |

## Workflow nodes

### Registration Form

Collects the student's registration information through an n8n Form Trigger.

### Code in JavaScript

Validates the submitted email and WhatsApp/phone number.

### If

Routes valid submissions toward registration processing and invalid submissions toward an error page.

### Config

Stores reusable program configuration such as the WhatsApp destination and program start date.

### Lookup Existing Registration

Checks Google Sheets for an existing registration using the submitted email address.

### Already Registered?

Determines whether the submitted email already exists.

### Show Already Registered

Displays a message telling an existing registrant that they do not need to register again and provides the WhatsApp group link.

### Save to Google Sheets

Stores the new registration.

### Send Confirmation Email

Sends an HTML confirmation email containing registration information and the WhatsApp group link.

### Show WhatsApp Group

Displays the final registration-success page and directs the student to the WhatsApp group.

## Security

**Do not commit production credentials, private invite links, API keys, passwords, or private infrastructure details to GitHub.**

This repository intentionally uses placeholders for environment-specific values.

Before publishing or updating the workflow, inspect the exported JSON for:

- Credential IDs
- Spreadsheet IDs
- Private URLs
- Webhook URLs
- API keys
- Authentication tokens
- Private community/group links

## Customization

The workflow was originally designed for Python From Zero. To reuse it for another program, update:

- Form title and description
- Form fields
- Program start date
- Confirmation email subject/body
- Success page
- Duplicate-registration page
- Google Sheet columns
- WhatsApp/community destination
- Branding/CSS

The core registration architecture can remain unchanged.

## Example use cases

This workflow can be adapted for:

- Online courses
- Free training programs
- Workshops
- Bootcamps
- Webinars
- Community onboarding
- Event registration
- Student applications
- Lead capture

## Design approach

The workflow follows a simple automation principle:

> **Validate → Check → Store → Notify → Redirect**

This keeps the registration process understandable and reduces unnecessary manual administration.

## Original project

This workflow was built for:

**Python From Zero — Free Beginner Program**

The original program used WhatsApp voice-chat sessions and collected beginner information before admitting participants to the community.

## License

MIT License. See [`LICENSE`](LICENSE).
