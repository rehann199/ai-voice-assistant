# Privacy and Source

## Source code

The production source code for AssistX is private and not included in this repository. This repository contains the architecture documentation, screenshots, and design overview for portfolio evaluation only. No licence to the source code is granted.

## Data privacy

**Local processing:** All patient queries, appointment bookings, and command logs are processed on the clinic's own server. No data is sent to cloud services except:
- Optional Google Speech-to-Text (only audio, if Whisper is unavailable and enabled by clinic admin)
- Optional Whisper audio processing (runs locally; no audio leaves the server)

**Demo data:** This portfolio uses entirely fictional patient data:
- All patient names are generic placeholders.
- Email addresses use `example.com` and phone numbers use the `555` prefix (reserved for fictional use).
- All dates are relative to first startup and do not represent real clinic schedules.
- Screenshots show demo data only; no real patient information is visible.

**Audit log:** The `voice_logs` table records every voice command transcription for compliance auditing. In production, this log would be restricted to authorized clinic staff and subject to data retention policies.

## Third-party components

- **FastAPI** – Python web framework (BSD 3-Clause License)
- **Bootstrap 5** – CSS framework (MIT License), loaded from CDN
- **NLTK** – Natural Language Toolkit (Apache 2.0 License)
- **OpenAI Whisper** – Speech-to-text model (MIT License, optional)
- **pyttsx3** – Text-to-speech library (MIT License, optional)
- **SQLite** – Database engine (public domain)

All open-source; see repository `requirements.txt` for full versions and licenses.

## Screenshots and assets

All screenshots on this page are captures from the demo build. The dashboard UI, color scheme, and styling are part of the portfolio showcase. Copying or reusing these assets requires permission.

## Deployment recommendations

For production clinic deployment:
- Store the database in a secure, backed-up location with access controls.
- Use HTTPS and authentication (not included in this demo) to prevent unauthorized command execution.
- Restrict API access to clinic network or VPN.
- Log all appointment changes and voice commands to a centralized audit system.
- Comply with HIPAA (USA), GDPR (EU), or local medical data regulations.
- Do not configure cloud STT fallback unless required and approved by legal.

This repository is a showcase only and does not include authentication, authorization, or encryption. A production deployment would require security hardening.
