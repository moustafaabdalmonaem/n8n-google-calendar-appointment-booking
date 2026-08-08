# Simple Appointment Booking System

An automated appointment-booking workflow built with [n8n](https://n8n.io). It receives a booking request from a form, checks Google Calendar for scheduling conflicts, and automatically confirms or rejects the appointment — no manual checking required.

## What it does

1. **Receives a booking request** through a webhook (e.g. from a website form) containing the customer's name, email, requested start/end time, and service type.
2. **Checks Google Calendar** for any existing events in that time range.
3. **Decides automatically**:
   - If the time slot is **free** → creates a new event in Google Calendar and sends a **confirmation email** to the customer.
   - If the time slot is **already booked** → sends a **rejection email** asking the customer to pick a different time.

## Workflow diagram

```
Booking Form (Webhook)
        │
        ▼
  Extract Booking Details
        │
        ▼
  Check Calendar for Conflicts (Google Calendar)
        │
        ▼
   Is Time Slot Free? (IF)
       │           │
      Yes          No
       │           │
       ▼           ▼
Create Calendar   Send Rejection
Event             Email
       │
       ▼
Send Confirmation
Email
```

## Requirements

- An [n8n](https://n8n.io) instance (cloud or self-hosted).
- A Google account with **Google Calendar API** access configured in n8n.
- A **Gmail** account connected in n8n (OAuth2 credentials) for sending confirmation/rejection emails.

## Setup instructions

1. Import `appointment-booking-workflow.json` into your n8n instance:
   `Workflows → Import from File`.
2. Open the **Check Calendar for Conflicts** and **Create Calendar Event** nodes and:
   - Set your Google Calendar credentials.
   - Replace `PUT_YOUR_GOOGLE_CALENDAR_ID_HERE` with your actual Calendar ID (your email address works for the primary calendar).
3. Open **Send Confirmation Email** and **Send Rejection Email** and set your Gmail credentials.
4. Activate the workflow.
5. Copy the webhook URL from the **Booking Form (Webhook)** node and use it as the submission endpoint for your booking form.

## Example webhook payload

```json
{
  "name": "Jane Doe",
  "email": "jane@example.com",
  "startTime": "2026-08-15T10:00:00+02:00",
  "endTime": "2026-08-15T10:30:00+02:00",
  "service": "Consultation"
}
```

## Customization ideas

- Add a list of available services with fixed durations, so the customer only picks a date and start time.
- Show available time slots to the customer before they submit (via a separate "check availability" endpoint) instead of rejecting after submission.
- Add SMS/WhatsApp confirmation in addition to email.
- Add a cancellation link in the confirmation email that removes the calendar event automatically.
- Support multiple calendars (e.g. per staff member) and route the booking to whichever one is free.

## License

Free to use and modify for personal or commercial projects.


<img width="1280" height="629" alt="Workflow 6" src="https://github.com/user-attachments/assets/3b4831da-825d-4d11-b5c3-e05eb9ce9bbe" />
