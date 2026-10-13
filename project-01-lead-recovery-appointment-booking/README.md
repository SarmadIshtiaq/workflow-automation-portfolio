# Lead Recovery & Appointment Booking Automation

A portfolio project demonstrating a low-code lead intake workflow built with Tally, n8n, HubSpot CRM, Slack, and a Calendly booking link.

> **Status: MVP / learning project — not production-ready.** This repository contains a sanitized, inactive n8n workflow draft. The public copy has had workspace credential references removed and a known validation-condition issue corrected, but the corrected export still needs to be imported into a test workflow and retested before describing that correction as verified.

## Problem

Small service businesses can lose opportunities when enquiries are not captured consistently, contact records are duplicated, or sales staff do not receive timely notifications.

## Intended flow

1. Tally collects a lead's name, email, company, service interest, budget range, and consent to be contacted.
2. An n8n Webhook receives the form-submission event.
3. `Normalize Lead Data` extracts fields, trims text, lowercases the email, and converts dropdown option IDs into readable option text where available.
4. `Validate Lead` checks that the name, email, service, and contact consent meet basic rules.
5. Valid leads are searched in HubSpot by email.
6. The HubSpot create-or-update action handles the contact using the email as the matching field.
7. If HubSpot reports `isNew: true`, an n8n IF node routes the new contact to a Slack notification that includes a Calendly booking link.
8. Invalid leads are transformed into an exception record by `Prepare Exception Record`.

## Tools

- Tally — lead intake form and webhook sender
- n8n Cloud — workflow orchestration
- HubSpot CRM — contact search and create/update
- Slack — new-contact notification
- Calendly — public booking page link

## Importing the sanitized workflow

1. In n8n, create a new workflow or use **Import from File** to import `workflows/workflowlabs-lead-recovery-portfolio-sanitized.json`.
2. The exported copy is inactive. Keep it inactive while configuring and testing.
3. Re-select/create your own credentials for the Webhook Header Auth, HubSpot, and Slack nodes. Credential references were deliberately removed from this public copy.
4. Set a webhook path you control and configure Tally to send to the newly generated test URL.
5. Select your own Slack channel in the Slack node.
6. Replace `REPLACE_WITH_YOUR_PUBLIC_CALENDLY_BOOKING_LINK` with your public event link.
7. Run the tests in the checklist below before activating the workflow.

## Security notes

- Never commit API keys, service keys, access tokens, webhook URLs, customer data, or raw execution payloads.
- This public export has its credential-reference objects and workspace-specific Slack channel selection removed. It is inactive and uses a placeholder webhook path and Calendly link.
- Treat webhook URLs as private and use request authentication. The shared header secret must be stored in n8n credentials and configured to match the sender. Do not put the secret in this repository.
- The workflow's current exception path only shapes an exception record; it does not yet persist errors to a database or send an exception alert.
- Basic email-format validation does not prove that an address exists or can receive mail.
- Use synthetic test data only in demos.

## Test evidence so far

These results are based on the builder's manual tests and reports, not automated CI results:

- Tally submission was received by the n8n Webhook and shown in the execution output.
- The normalization Code node returned structured lead fields and readable dropdown values.
- A valid synthetic test lead passed the basic validation checks.
- A synthetic contact was created in HubSpot.
- A repeat test using the same email returned `isNew: false`; the builder then reported that only one matching contact was present in HubSpot.
- The Slack message action sent a notification to the selected channel with lead fields and a Calendly booking link.

These are preliminary manual test observations. No 30-record benchmark or aggregate performance metrics have been calculated yet.

## Known gaps / next improvements

- Import and retest the corrected `isValid` condition in a separate workflow before publishing it as tested.
- Add persistent exception logging and error alerts.
- Add retry/backoff and idempotency handling for repeated webhook event IDs.
- Validate consent, required fields, malformed email, missing fields, duplicate submissions, and downstream API failures using controlled tests.
- Build at least 30 synthetic test records and calculate metrics from actual run outputs.
- Add scheduled follow-up reminders with a durable follow-up status and a duplicate-reminder guard.
- Calendly booking/cancellation/reschedule event automation is not implemented in this workflow export. It currently shares a booking link only.
- Add test evidence (sanitized screenshots or a short demo) and a reproducible test-results table.

## Suggested repository structure

```text
business-automation-portfolio/
└── project-01-lead-recovery-appointment-booking/
    ├── README.md
    ├── workflows/
    │   └── workflowlabs-lead-recovery-portfolio-sanitized.json
    ├── docs/
    │   └── SECURITY_CHECKLIST.md
    
```

## License

Add a license only when you decide how you want others to reuse this portfolio project.
