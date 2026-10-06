# Engineering Challenges

## Challenge 1: Offline NLP without an LLM

**Problem:** Running a large language model locally requires GPU memory and adds startup latency. Cloud APIs incur latency, cost, and privacy risks in a clinic setting.

**Solution:** Built a lightweight, deterministic command parser using regex and NLTK utilities. A command like "Show last visit for Ahmed" is split into tokens, stopwords are removed, and verbs and entities (patient names, dates, doctor roles) are extracted via pattern matching. Intent (query, list, book) is inferred from the verb and context.

**Trade-off:** The system cannot learn new phrasings without code changes. Staff must use consistent command patterns. In practice, this works well for a single clinic where training is simple.

## Challenge 2: Handling missing optional dependencies

**Problem:** Installing Whisper, Google Speech-to-Text libraries, and pyttsx3 adds 500+ MB and requires system audio libraries (FFmpeg, portaudio). A developer machine or CI/CD pipeline may not have these.

**Solution:** Each voice module imports with try/except; if a dependency is missing, the feature reports "unavailable" to the UI rather than crashing. Users can still use typed commands. The Docker build includes all dependencies; lightweight deployments skip voice by design.

## Challenge 3: FastAPI version pinning

**Problem:** FastAPI 0.105+ uses Starlette 0.27+, which changed the `TemplateResponse` API. The application's `TemplateResponse(request, template, context)` call breaks in newer versions.

**Solution:** Pinned `fastapi==0.104.1` in `requirements.txt`. A full migration to the new API would require refactoring Jinja2 template rendering; deferred to preserve the original source.

## Challenge 4: Browser microphone capture and permissions

**Problem:** The browser's `MediaRecorder` API requires user permission to access the microphone. Permission state varies by browser, HTTPS requirements, and user settings.

**Solution:** The UI checks `navigator.mediaDevices` and prompts for permission on first use. If denied, voice input is disabled gracefully. Status cards indicate when voice is ready. In the demo build, browser sandbox restrictions prevent testing; the code works in production.

## Challenge 5: Audit trail for voice interactions

**Problem:** Clinics must log who said what and when, for compliance and debugging. Voice commands are ephemeral without a record.

**Solution:** Every voice command (transcribed text) is logged to a `voice_logs` table with timestamp, user agent, and result. The command log UI shows recent interactions. This enables post-hoc review and debugging.

## Challenge 6: Demo data realism

**Problem:** Showing a screenshot with stale dates (e.g. appointments in 2025) undermines credibility. Real data is private.

**Solution:** The seeding script uses relative dates (today, tomorrow, next week) computed at startup. The committed database was updated with current-day appointments and visits for the portfolio. Reverting to the backup DB restores the original state.

## Challenge 7: Single-page responsiveness

**Problem:** A desktop single-page app is not responsive to all screen sizes. Staff may use tablets or phones.

**Solution:** Bootstrap 5 grid and flexbox layout. Modals and tables are mobile-friendly. Mic input and buttons scale to touch targets (48+px). Not tested on all devices; a production build would include responsive testing.

Continue with [privacy and source](PRIVACY_AND_SOURCE.md).
