# HalbiSetu — Phased Task List (Jira-style)

Format per item: **ID — Title** · description · acceptance criteria.
Epics map to the milestones in the main build prompt (M1–M7).

---

## EPIC 1 — Project foundation (pre-M1)
- **HB-1 — Repo & stack setup.** Monorepo with `core/`, `api/`, `web/`, `android/`,
  `desktop/`. Flutter shared core (Android+Windows) + web/PWA, FastAPI backend,
  Postgres, Meilisearch, Redis, object storage. CI on every PR.
  *AC:* A hello-world builds on all three targets; CI green; README lists stack + Section-2 caveats.
- **HB-2 — Environments & secrets.** Dev/staging/prod config; no hardcoded keys.
  *AC:* Secrets loaded from env/secret store; `.env.example` committed.
- **HB-3 — Data-model migration.** Implement `Language, Entry, Sense, Translation,
  AudioClip, ExampleSentence, Submission, Revision, SearchLog, User`.
  *AC:* Migrations run clean; schema doc generated.

## EPIC 2 — Lexicon MVP (M1)
- **HB-10 — Seed lexicon.** Import a licensed/community Halbi wordlist (~2,000 words),
  verify licence, map to entry model.
  *AC:* ≥2,000 entries loadable; source + licence recorded per entry.
- **HB-11 — Search API.** `GET /search?q=&lang=&target=` with fuzzy match.
  *AC:* Results <300 ms at 10k entries; returns entries, translations, audio, examples.
- **HB-12 — Dictionary display (web).** Typed search + entry page showing all five
  languages, POS, senses, examples.
  *AC:* Any of the 5 languages works as query language; entries render correctly.
- **HB-13 — Status labels.** Every translation carries `verified|community|ai_suggested|unverified`.
  *AC:* Labels visible in UI; no unlabelled AI output possible.

## EPIC 3 — Voice: speaking & listening (M2)
- **HB-20 — SpeechService abstraction.** One interface wrapping TTS + STT providers
  so they can be swapped.
  *AC:* Provider change requires no UI code change; unit-tested with a mock.
- **HB-21 — TTS on entries.** Speaker button on every word/definition/sentence;
  speed control; priority human→neural→phonetic fallback.
  *AC:* Playback starts <1 s; cached audio replays instantly.
- **HB-22 — Audio cache & offline.** Cache generated audio locally on mobile/desktop.
  *AC:* Replayed audio works with network off.
- **HB-23 — Voice search (STT).** Mic button, live partial transcript, language
  auto-detect with override; cloud STT for supported languages.
  *AC:* Works on web, Android, Windows with correct permissions.
- **HB-24 — Halbi phonetic fallback.** Fuzzy-match Halbi speech against lexicon;
  return "Did you mean…" candidates.
  *AC:* Given a spoken Halbi word, returns ranked candidates from the lexicon.
- **HB-25 — Community audio library.** Contributors record pronunciations; stored
  with `source=human`, quality rating.
  *AC:* Human clips take priority over TTS when present.

## EPIC 4 — AI layer (M3)
- **HB-30 — AI sentence generation.** `POST /ai/sentences` → 3–5 example sentences
  per word, in target language, with translation back to user's language.
  *AC:* Each sentence labelled `AI-suggested`; unknown word returns "not in lexicon", no invented meaning.
- **HB-31 — Fallback translation.** When no dictionary pair exists, call LLM;
  label result `ai_suggested`.
  *AC:* Fallback never overrides a verified dictionary entry.
- **HB-32 — Voting on AI output.** Up/down vote + edit on generated sentences.
  *AC:* Votes stored and surfaced to admins; edits create revisions.
- **HB-33 — Cost controls.** Rate-limit AI + speech endpoints; cache responses.
  *AC:* Abuse protection in place; repeat requests served from cache.

## EPIC 5 — Contributions & moderation (M4)
- **HB-40 — Submission endpoints.** New words, corrections, examples, audio.
  *AC:* All submissions enter `pending`; nothing goes live unreviewed.
- **HB-41 — Review queue (admin).** Approve/reject with reviewer + timestamp.
  *AC:* Approved items become `community`; rejections notified to submitter.
- **HB-42 — Trust levels.** anonymous → registered → trusted → admin privileges.
  *AC:* Privileges enforced server-side; trusted users can approve in their language.
- **HB-43 — Revision history.** Every change stored with diff, actor, timestamp;
  rollback supported.
  *AC:* Any entry's full history viewable and revertible.
- **HB-44 — Search logging.** Anonymised logs + "requested words" queue of misses.
  *AC:* Admin sees top requested-but-missing words.

## EPIC 6 — Platforms: Android & Windows (M5)
- **HB-50 — Android build.** Shared core → APK/AAB; offline dictionary + cached audio;
  background sync of contributions.
  *AC:* Installs and works offline; syncs on reconnect.
- **HB-51 — Windows build.** Native window, system tray, global hotkey for translate bar,
  on-disk cache, auto-update.
  *AC:* Hotkey opens translate bar; updates apply cleanly.

## EPIC 7 — Global translate bar & PWA (M6)
- **HB-60 — Global translate bar.** Compact bar pinned top-right on web, Android, Windows;
  source/target dropdowns; text + voice input; speaker button; minimisable, remembers state.
  *AC:* Present on every screen, all three platforms; doesn't block main UI.
- **HB-61 — PWA / offline hardening.** Installable web app; core lookup offline.
  *AC:* Installable; core dictionary usable offline in browser.

## EPIC 8 — Learning loop (M7)
- **HB-70 — Analytics pipeline.** Aggregate anonymised search signals for model + lexicon priorities.
  *AC:* Weekly report of demand vs. coverage.
- **HB-71 — Fine-tuning job.** Periodic offline retraining of AI/speech models from
  approved contributions.
  *AC:* Documented, reproducible job; improvements measured on a held-out set.

## EPIC 9 — Cross-cutting / non-functional
- **HB-80 — Accessibility.** Large targets, screen-reader labels, text scaling, high contrast.
- **HB-81 — UI i18n.** App UI translatable; default follows device, switchable to Hindi/Marathi/English.
- **HB-82 — Privacy & consent.** Voice stored only with explicit opt-in; logs anonymised.
- **HB-83 — Security.** Auth, input validation, rate limits, audit log on data changes.
- **HB-84 — Documentation.** Setup, schema, adding a voice/provider, learning-loop retraining, README caveats.
