# Data sources

HalbiSetu bundles dictionary data from several sources. Each has its own origin and licence — please read this before reusing the data.

## 1. SIL International — Halbi Dictionary (primary target source)

- **Publisher:** SIL International (Webonary)
- **Compiler:** Frances M. Woods
- **Edition:** Self-reviewed draft, 2019
- **Extent:** ~7,556 entries
- **Languages:** Halbi [hlb], Hindi [hin], English [eng]
- **Scripts:** Devanagari, Latin
- **Online:** https://www.webonary.org/halbi/
- **Status:** *Not yet imported into this repo.* This is the recommended primary dataset; confirm terms with SIL before bulk redistribution.

## 2. Swadesh Word List — Halbi (seed data)

- **Publisher:** SEL India / SIL
- **Content:** ~100 core vocabulary items (English · Hindi · Halbi IPA)
- **Use here:** seed entries (`status: demo`)

## 3. हिन्दी–हल्बी शब्दकोश (OCR-extracted)

- **Publisher:** Tribal Research & Training Institute (आदिमजाति अनुसंधान एवं प्रशिक्षण संस्थान), Raipur, Chhattisgarh
- **Year:** 2015–16
- **Content:** ~1,200 Hindi→Halbi entries in a three-column table (क्र. | हिन्दी | हल्बी)
- **Use here:** extracted by OCR from a ~200 dpi scan; entries carry `status: ocr` and **contain errors**.
- **Licence:** Government of Chhattisgarh publication — verify reuse terms before redistributing.

## 4. Public Hindi→Halbi word lists

- Small community word lists (e.g. Jagdalpura) used as seed data (`status: demo`).

## Generated glosses

Marathi and Sanskrit translations for the seed words were produced automatically by a translation engine and have **not** been verified by speakers. Treat them as drafts.

---

## How to cite in the app

Every entry carries a `source` string and a `status` value (`demo`, `ocr`, or `community` for user corrections). Keep these accurate when adding data — they are the project's audit trail.
