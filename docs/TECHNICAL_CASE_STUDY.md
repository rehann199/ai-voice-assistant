# Technical Case Study

## Problem

Medical clinics need a fast, hands-free way for staff to query patient records and manage appointments. Typing takes time when staff are busy; voice commands with local processing avoid cloud delays and privacy concerns. An offline-first system ensures HIPAA and data residency compliance.

## Solution

AssistX combines a local NLP interpreter with a web-based dashboard. Commands are processed locally without external API calls (except optional Google STT fallback). Database queries return results within milliseconds; voice synthesis plays responses immediately.

## Implementation choices

**Command understanding (rule-based, not LLM):** Regex patterns match verbs and intent, entity extraction uses NLTK utilities and dateutil for date parsing. This avoids LLM latency and cost, works offline, and is deterministic and auditable. Trade-off: does not handle complex paraphrasing as well as a trained model, but sufficient for medical clinic workflows where staff learn to phrase commands consistently.

**Voice I/O (optional, graceful degradation):** Whisper base model runs locally if installed; Google STT is a fallback. pyttsx3 provides offline text-to-speech. If dependencies are missing, typed commands still work fully. This allows the system to run on light environments (development, CI/CD) without downloading large models.

**FastAPI + SQLite:** FastAPI is lightweight and fast for REST endpoints. SQLite is zero-setup, suitable for single-clinic deployments. For multi-tenant or high-concurrency scenarios, a centralized database would be needed.

**Single-page HTML + Vanilla JS:** No frontend build step, minimal dependencies. Bootstrap 5 provides responsive UI; vanilla JavaScript keeps the bundle small.

**Docker deployment:** Dockerfile and docker-compose enable one-command deployment and portability.

## Performance and limits

- Command interpretation: <100 ms for regex matching and entity extraction.
- Database queries: <50 ms for typical patient/appointment searches (SQLite on modern hardware).
- Voice transcription: 1–5 seconds for audio segments (Whisper base model on CPU) or immediate (Google fallback).
- Dashboard: ~2 MB total (HTML + CSS + JS + static assets); loads in <1 second on broadband.

## Testing

Unit tests cover database operations, NLP intent matching, and API response formats. All data is seeded; no external services are mocked.

## Known limitations

- **Appointment booking bug:** Voice commands to book appointments fail due to a regex entity-capture bug (not fixed to preserve source integrity). Text-based booking POST requests work correctly.
- **Voice readiness:** The status card reflects only server health, not actual microphone or STT availability. Users must check browser console or network logs to diagnose voice failures.
- **Seed dates:** On first run, demo data uses dates relative to startup time. A stale committed database would show past appointments; the tool reseeds on first load if needed.
- **CDN dependencies:** Bootstrap and icon fonts load from CDN; offline deployment needs local mirrors.

Continue with [engineering challenges](ENGINEERING_CHALLENGES.md).
