# BUILD PROMPT — "HalbiSetu": Multilingual Halbi Dictionary with Voice & AI

## 0. How to use this prompt
Paste this whole document as the brief for an AI coding agent or hand it to a
development team as the product + technical specification. It is written so that
an agent can scaffold the project end-to-end. Where a decision is left open, the
"Defaults" note states the choice to make unless told otherwise.

---

## 1. Mission
Build a dictionary and translation application for **Halbi** (ISO 639-3: `hlb`),
an Indo-Aryan language spoken mainly in the Bastar region of Chhattisgarh and in
neighbouring parts of Odisha, Maharashtra and Andhra Pradesh (several hundred
thousand speakers). Halbi is written primarily in **Devanagari**.

The app must let a user look up any word or sentence and:
1. See its meaning and translation across **Halbi, Marathi, Hindi, Sanskrit and English**.
2. **Hear** any word or sentence spoken aloud (text-to-speech) in any of the five languages.
3. **Speak** a word or sentence and search by voice (speech-to-text).
4. Get **AI-generated example sentences** that use the looked-up word correctly.
5. **Contribute** corrections, new words, pronunciations and example sentences —
   the system learns from every user search and submission over time.
6. Use a **global translation bar** pinned to the top corner of every screen, on
   every platform, for instant one-off translations.

Deliver it as a **web app**, an **Android app**, and a **Windows desktop app**.

---

## 2. Non-negotiable reality check (read before choosing the stack)
Halbi is a **low-resource language**. There is **no off-the-shelf** translation
model, TTS voice, or STT model for Halbi today. Therefore:

- **Dictionary lookup must be data-driven first** (a curated lexicon), with AI
  translation used only as a *fallback* and always labelled as AI-generated and
  unverified.
- **TTS for Halbi** will not exist in commercial APIs. Plan a hybrid:
  (a) ship a phoneme/phonetic fallback by rendering Halbi text through the closest
  available Devanagari voice (Hindi/Marathi) as a stopgap, and
  (b) build a community **recorded-audio library** (human pronunciations) that
  becomes the primary source as it grows.
- **STT for Halbi** will not be supported by Google/Azure. Plan to use a
  multilingual model (e.g. Whisper) for the supported languages, and a
  **phonetic/syllable fuzzy-match** layer for Halbi queries, with a fine-tuning
  path once enough community audio exists.
- **Sanskrit** has good TTS/STT coverage in some engines but weaker in others —
  verify per provider.
- Never present an unverified AI translation as authoritative. Always show a
  confidence/status label: `Verified`, `Community`, `AI-suggested`, `Unverified`.

State these constraints visibly in the product (a small info note), not just in code.

---

## 3. Target users & core use cases
- **Speakers & learners** wanting to translate between Halbi and the four languages.
- **Students/teachers** in the Bastar region learning Halbi or standard Hindi/Marathi.
- **Researchers/linguists** collecting and checking lexical data.
- **Travellers/officials** needing quick Halbi↔Hindi/English translation.

Primary flows:
1. Type or speak a word → get definitions, translations in all 5 languages, audio, and example sentences.
2. Type or speak a full sentence → get a translation and hear it.
3. Tap the global translate bar anywhere → quick translate without leaving the screen.
4. Contribute: add a missing word, fix a translation, record a pronunciation, rate an AI sentence.

---

## 4. Functional requirements

### 4.1 Dictionary core
- Search across the five languages; any language can be the query language.
- Entry model per word: headword, language, script (Devanagari / Latin
  transliteration / optional Odia script), part of speech, senses, definitions,
  synonyms, antonyms, example sentences, audio clips, dialect/region notes,
  source & verification status.
- Bidirectional lookup: Halbi→{Marathi, Hindi, Sanskrit, English} and back.
- Handle morphology lightly: show related forms (plural, verb conjugations,
  gender variants) where known.
- Offline-capable core dictionary on mobile and desktop (bundled + sync).

### 4.2 Voice — speaking (TTS)
- A speaker button on every word, definition and sentence.
- Language selector for the voice.
- Priority order for audio: **human recording → AI/neural TTS → phonetic fallback**.
- Speed control (0.5×–1.5×), and a "spell it out" mode for pronunciation practice.
- Cache generated audio so repeated requests are instant and offline-available.

### 4.3 Voice — listening (STT)
- A microphone button for voice search in all five languages.
- Live partial transcription while speaking.
- For Halbi: fall back to phonetic fuzzy-matching against the lexicon and offer
  "Did you mean…" candidates.
- Auto-detect language where possible; let the user override.
- Works on web, Android and Windows (permissions handled per platform).

### 4.4 AI sentence generation
- For any looked-up word, generate 3–5 natural example sentences **in the target
  language**, each with a translation back to the user's language.
- Show part of speech and usage context; mark as `AI-suggested`.
- Allow the user to upvote/downvote/edit; feed signals back to the model.
- Guard against hallucination: if the word is unknown to the lexicon, say so and
  do not invent a meaning.

### 4.5 Learning from users (the feedback loop)
This is a first-class feature, not an afterthought.
- Every search is logged (anonymised) as a signal of demand — missing words rise to the top of a "requested words" queue for admins.
- Users can submit: new words, corrected translations, pronunciation recordings,
  example sentences, and ratings on existing entries.
- A **moderation/review queue**: submissions are `pending` until a trusted
  contributor or admin approves → then `Community` or `Verified`.
- Contributor trust levels (anonymous → registered → trusted → admin) with
  different privileges.
- A periodic (offline) retraining/fine-tuning job uses approved contributions to
  improve the AI translation and TTS/STT models.
- Store every change as an auditable revision (who, when, what, before/after) so
  bad edits can be rolled back.

### 4.6 Global translation bar
- A compact bar pinned to the **top-right corner** of every screen on web, Android
  and Windows.
- Source & target language dropdowns (default: auto-detect → English, or the
  user's last pair).
- Type or paste text, or use voice input, → instant translation with a speaker button.
- Collapsible/minimisable so it never blocks the main UI; remembers last state.

### 4.7 Accounts & admin
- Optional sign-in (email / Google) to contribute; anonymous browsing allowed.
- Admin console: manage the lexicon, the review queue, audio library, users,
  trust levels, and export/import data (CSV/JSON).

---

## 5. Platform requirements

### 5.1 Web app
- Responsive (desktop + mobile browsers). PWA so it installs and works offline.
- Mic access via Web Speech API where available, with a cloud STT fallback.

### 5.2 Android app
- Native-feeling, works offline with bundled dictionary + cached audio.
- Background sync of contributions when connectivity returns.
- Proper runtime permission handling for mic and storage.

### 5.3 Windows desktop app
- Native window, system tray, global hotkey to open the translate bar.
- Offline dictionary + audio cache on disk; auto-update mechanism.

**Default stack recommendation (choose unless told otherwise):**
- **Shared core:** a single Flutter codebase targeting **Android + Windows** (and
  optionally web), OR a React web app + React Native (Android) + Tauri/Electron
  (Windows). Prefer **Flutter** if one team must ship all three fast; prefer the
  JS stack if web quality is the priority.
- **Backend:** Python (FastAPI) or Node (NestJS) REST/GraphQL API.
- **Database:** PostgreSQL for the lexicon + revisions; object storage for audio;
  a search index (Meilisearch/Elasticsearch) for fast fuzzy lookup; Redis cache.
- **AI layer:** an LLM API for sentence generation and fallback translation, plus
  an offline-capable local model option (e.g. a small multilingual model) for
  privacy/offline modes.
- **Speech:** cloud TTS/STT for the four supported languages; Whisper-family model
  self-hosted for STT; a phonetic engine for Halbi. Abstract all of these behind a
  single `SpeechService` interface so providers can be swapped.

---

## 6. Data model (starting point)
- `Language` (code, name, script(s), direction)
- `Entry` (id, headword, language, script_form, transliteration, pos, status, dialect, source, created_by, created_at)
- `Sense` (entry_id, definition, examples, synonyms, antonyms, order)
- `Translation` (source_entry_id, target_language, target_text, confidence, status, contributor_id)
- `AudioClip` (entry_id or sentence_id, language, url, source=human|tts, quality, recorded_by)
- `ExampleSentence` (entry_id, language, text, translation, source=ai|human, votes)
- `Submission` (type, payload, submitted_by, status=pending|approved|rejected, reviewer_id, reviewed_at)
- `Revision` (entity, entity_id, diff, actor_id, timestamp)
- `SearchLog` (query, detected_language, result_found, timestamp) — anonymised
- `User` (id, role/trust_level, languages)

Seed the lexicon with any openly available Halbi wordlists (verify licensing),
starting with the most common ~2,000 words and expanding via contributions.

---

## 7. API surface (illustrative)
- `GET /search?q=&lang=&target=` → entries + translations + audio + examples
- `GET /translate?text=&source=&target=` → translation with `status` label
- `POST /speech/tts` (text, language) → audio
- `POST /speech/stt` (audio, language?) → transcript
- `POST /ai/sentences` (word, language, count) → example sentences
- `POST /contributions` → create a pending submission
- `POST /contributions/{id}/review` → approve/reject (privileged)
- `GET /entries/{id}/revisions` → change history
- Auth, admin and export endpoints as needed.

Every translation/sentence response must carry: `text`, `source` (dictionary | ai),
`status` (verified | community | ai_suggested | unverified), `confidence`.

---

## 8. Non-functional requirements
- **Performance:** search results < 300 ms; TTS playback starts < 1 s (cached: instant).
- **Offline:** core lookup + cached audio work with no network on mobile/desktop.
- **Accessibility:** large tap targets, screen-reader labels, adjustable text size,
  high-contrast mode — important for a language-preservation tool.
- **Privacy:** search logs anonymised; voice audio only stored with explicit
  consent (and used only for model improvement if the user opts in).
- **Security:** standard auth, input validation, rate limiting on AI/speech endpoints
  (they cost money), audit log on all data changes.
- **Internationalisation:** the app UI itself should be translatable; default UI
  language follows the device, switchable to Hindi/Marathi/English.

---

## 9. Build milestones (suggested order)
1. **M1 — Lexicon MVP:** data model, seed data, search API, web app with typed search and dictionary display. No voice/AI yet.
2. **M2 — Voice:** TTS on all entries; STT voice search; audio caching.
3. **M3 — AI:** sentence generation + fallback translation, with status labels.
4. **M4 — Contributions:** submission + review queue + revisions + trust levels.
5. **M5 — Platforms:** Android and Windows apps sharing the core.
6. **M6 — Global translate bar** on all three platforms; PWA/offline hardening.
7. **M7 — Learning loop:** search analytics, requested-word queue, periodic model fine-tuning from approved data.

---

## 10. What to hand back / definition of done
- Working web app + Android build + Windows build from one shared core where feasible.
- A seeded Halbi lexicon with the five-language lookup, audio on entries, and the
  global translate bar on every platform.
- AI sentence generation with clear `AI-suggested` labelling and voting.
- A functioning contribution + moderation pipeline with revision history.
- Documentation: setup, data schema, how to add a language voice/provider, and
  how the learning loop retrains.
- A short README stating the low-resource caveats from Section 2 so future
  maintainers don't mistake AI output for verified data.

---

## 11. Open questions to confirm before M2/M3
1. Which commercial TTS/STT providers are acceptable (cost vs. quality vs. data
   residency for Indian languages)?
2. Is there an existing Halbi lexicon or corpus we can license, or do we start
   from community contributions only?
3. Should Sanskrit be full bidirectional translation, or reference-only
   (since Sanskrit↔Halbi direct pairs are scarce)?
4. Hosting: cloud, or self-hosted/on-prem (affects offline + privacy design)?
5. Who are the trusted reviewers for the moderation queue?
