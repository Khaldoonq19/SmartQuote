# SmartQuote - contribution and code walkthrough

## Problem and context

Customers need to describe a support, incident or enhancement request clearly enough for administrators to review it and prepare a quote. SmartQuote connects the customer submission workflow to the wider team's quoting and administration system.

This was a university team project for GIACOM. Khaldoon Qasim's contribution focused on the React/TypeScript frontend, including login, dashboards, ticket submission and administration screens, plus requirements, user stories and testing. He coordinated Sprint 2. These contributions describe the university project; the current repository also includes changes and work by teammates.

## A concrete workflow: submitting a ticket

1. The user enters the title, description, impact, deadline and optional attachments.
2. The form validates the input before making a request. The current implementation requires at least 15 title characters and 50 description characters, and checks the date, integer input and attachment limits.
3. An asynchronous hook sends the typed request through the API adapter.
4. The UI distinguishes creating the ticket from uploading attachments, and displays errors rather than silently treating a failed request as success.
5. On success, the form calls its parent callback so the surrounding view can update.

### Read the current code

- [Ticket form and submission state](../src/client/features/customer/SubmitTicketForm.tsx)
- [Typed asynchronous creation hook](../src/client/hooks/tickets/useCreateTicket.ts)
- [API adapter](../src/client/lib/api/ticket.api.ts)
- [Request/response contracts](../src/shared/contracts/ticket-contracts.ts)
- [Login page](../src/client/pages/misc/LoginPage.tsx)
- [Example repository smoke tests](../tests/e2e/smoke/ticket.smoke.test.ts)

These links are a guide to the current team implementation, not a claim that every linked line was authored by Khaldoon. Existing tests are part of the repository; no fresh full-suite result is claimed here.

## Engineering decisions worth discussing

- **Separate form, hook and API adapter:** presentation, request state and endpoint details have distinct responsibilities.
- **Typed contracts:** frontend and backend share the expected request shape. Runtime validation is still necessary; TypeScript alone cannot validate an incoming HTTP request.
- **Feedback during asynchronous work:** the form tracks submission phases. UI controls can discourage repeat submissions, but server-side idempotency is a separate reliability concern.
- **Partial failure:** ticket creation and attachment uploads are separate operations. If an upload fails after ticket creation, recovery needs to avoid accidentally creating another ticket. This is a useful next improvement to investigate, not a solved guarantee.

## Suggested two-minute demonstration

Use a local development environment and fictional data only. Follow the root README and project database guide to set it up; do not display real credentials or client records.

- Show the customer dashboard and navigate to the ticket form.
- Submit an incomplete form and explain the validation feedback.
- Enter a valid fictional ticket and show the submitting/success behaviour.
- Show the administrator view if a local demo account is available.
- Open the form, hook and shared contract to explain how the request moves through the application.

The project's published demo link is https://smartquote.zayeer.dev. This documentation review did not verify a live authenticated session or record a video.

## What I would improve next

Evaluate accessible error announcements and keyboard navigation, test failure/retry paths, and investigate safe recovery after partial attachment failure. For interview discussions, distinguish proposed improvements from implemented behaviour.
