# Contributing to HalbiSetu

Thank you for helping document and preserve Halbi. There are two ways to contribute.

## 1. Correct words in the app (no coding needed)

This is the most valuable contribution, and it needs someone who actually speaks Halbi.

1. Open the app (`index.html`, or the published GitHub Pages site).
2. Find a word and press **Edit**.
3. Fix any field — Halbi, Hindi, Marathi, Sanskrit, English — and press **Save**.
4. When you are done, press **Export** to download your corrections as a CSV/JSON file.
5. Attach that file to a GitHub issue, or email it to the maintainers, so the fixes can be merged into the main dataset.

Entries marked **OCR** came from a machine scan and are the most likely to be wrong — please check those first.

## 2. Improve the code or data (developers)

1. Fork the repository and create a branch: `git checkout -b fix/some-word`.
2. The app is a single file, `index.html` — no build step. Edit it and reload the browser.
3. The dataset lives inline in `index.html` (the `const DATA = [...]` array) and is mirrored in `data/halbi-dictionary.csv`.
4. Commit with a clear message and open a pull request.

### Data rules

- **Do not invent words.** Only add or change a translation if you can point to a source or you are a native speaker.
- Keep the source/status fields honest: use `OCR` for machine-extracted, `demo` for seed data, and let corrections show as `community`.
- Respect the original sources' licences (see README).

### Reporting issues

Use GitHub Issues. For a wrong translation, please include: the entry, what it currently says, what it should say, and your source or speaker background.

---

By contributing you agree your code contributions are licensed under the project's MIT licence, and your dictionary corrections may be published under the same terms as the rest of the dataset.
