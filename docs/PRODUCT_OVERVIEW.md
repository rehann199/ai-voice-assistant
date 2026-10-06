# Product Overview

All screenshots on this page are from a demo build running on seeded fictional data.

## What it is

AssistX is a voice-command medical clinic dashboard. Clinic staff type or speak commands to query patients and manage appointments. A single-page web interface displays patient records, upcoming appointments, visit history, and a command log. No cloud calls; all data stays local.

## Main interface

The dashboard offers five interactive sections:

- **Voice Assistant.** A text or voice input box where staff enter commands like "Show last visit for Ahmed" or "List today's appointments". The system interprets intent, extracts entities (patient names, dates, doctor names), and returns results in a table or card below.
- **Upcoming Appointments.** A sortable table of scheduled appointments with patient name, doctor, time, and status. Users can click to view details or cancel.
- **Patient Directory.** A searchable list of all patients. Clicking a patient opens a modal with full record: contact, recent visits, and upcoming appointments.
- **Voice Status.** Cards showing health, connectivity and readiness to accept voice input (demo build indicates availability only; microphone and speech-to-text require optional dependencies).
- **Command Log.** Recent commands typed or spoken, with timestamps and results for audit and learning.

## Commands

The system handles natural language like:
- "Show last visit for [patient name]"
- "List appointments for [doctor name]"
- "Show today's schedule"
- "Book an appointment for [patient]"

Intent and entity extraction use rule-based regex and NLTK, not an LLM.

## Data

Startup auto-seeds the database with fictional demo patients, appointments, and visits using email.com addresses and 555 phone numbers. All dates are relative to first run; no real patient data is stored.

Continue with the [architecture](ARCHITECTURE.md).
