# HalbiSetu — हल्बी शब्दकोश

An open, **community-editable multilingual dictionary** for **Halbi** (ISO 639-3: `hlb`), the Indo-Aryan language of the Bastar region of Chhattisgarh, India.

It works as a single self-contained web page — no server, no build step, no dependencies — and runs as an installable **PWA** on desktop, Android and Windows.

---

## Features

- **Multilingual lookup** across **Halbi · Hindi · Marathi · Sanskrit · English**.
- **Voice out (TTS):** a 🔊 button on every word, using the browser's speech engine. Falls back to the Hindi voice for Marathi/Sanskrit when no dedicated voice exists.
- **Voice in:** type or use the device's dictation/keyboard to search.
- **Native-speaker editing:** every entry has an **Edit** button. Corrections are saved in the browser and marked "corrected" — the core of the project's data-quality model.
- **Export / Import:** download the whole dictionary as **CSV** or as a **PDF** (via the print dialog → *Save as PDF*), and load corrections back from JSON.
- **Pagination:** 20 / 50 / 100 entries per page, with page navigation and jump-to-page.
- **Offline-first PWA:** install it and it keeps working without a network.

---

## Quick start

Just open the app:

```
# clone
git clone https://github.com/<your-user>/halbisetu.git
cd halbisetu

# open directly …
open index.html          # macOS
xdg-open index.html      # Linux
start index.html         # Windows
```

Or serve it locally (recommended, so the service worker and PWA install work):

```
python3 -m http.server 8080
# then visit http://localhost:8080
```

---

## Deploy (GitHub Pages)

1. Push this repository to GitHub.
2. **Settings → Pages → Build and deployment → Source: GitHub Actions** (a workflow is included at `.github/workflows/pages.yml`).
3. Every push to `main` publishes the site. Your app will be at
   `https://<your-user>.github.io/halbisetu/`

---

## Data sources & credits

HalbiSetu stands on the work of the people who documented this language. Please respect their licences.

| Source | What it provides | Note |
|---|---|---|
| **SIL International — Halbi Dictionary** (Webonary, comp. Frances M. Woods, 2019, ~7,556 entries) | Halbi–English–Hindi lexicon | © SIL International. The intended primary dataset; check its terms before redistributing. |
| **SIL / SEL India — Swadesh Word List (Halbi)** | ~100 core words (Halbi IPA + Hindi + English) | Seed data. |
| **हिन्दी–हल्बी शब्दकोश**, Tribal Research & Training Institute (TRTI), Raipur, 2015–16 | ~1,200 Hindi–Halbi entries | Government of Chhattisgarh publication. Entries in this repo were **OCR-extracted and are noisy** — verify before relying on them. |
| **Public Hindi→Halbi lists** (community websites) | Small word lists | Seed data. |

Marathi and Sanskrit glosses for the seed words were generated automatically and should be reviewed by speakers.

> **Accuracy disclaimer.** Entries flagged `OCR` were machine-extracted from a low-resolution scan and can contain errors, especially in the Halbi column. They are provided as a starting point for correction, **not** as an authoritative lexicon. Always prefer the SIL dataset where available.

---

## Roadmap

- [ ] Import the full SIL Halbi dictionary (7,556 entries) as the primary dataset.
- [ ] Crowd-correction backend so edits sync across devices (currently they stay in the browser).
- [ ] **Android APK** — wrap this PWA with [Capacitor](https://capacitorjs.com/) and build a signed APK.
- [ ] Windows desktop build (same Capacitor/Tauri approach).
- [ ] Recorded native-speaker audio for Halbi pronunciations.

---

## Contributing

Corrections from native speakers are the most valuable contribution. See [CONTRIBUTING.md](CONTRIBUTING.md).

---

## Licence

- **Code:** MIT — see [LICENSE](LICENSE).
- **Dictionary data:** belongs to its original sources (see the table above). The MIT licence covers the application code only, **not** the lexical data.

---

*Built for the Halbi community of Bastar.*
