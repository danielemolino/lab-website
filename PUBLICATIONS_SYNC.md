# Publications Sheet Sync

This workflow lets colleagues edit a shared spreadsheet and then regenerate the site bibliography automatically.

## Where the configuration lives in the repo

The sync configuration file is:

`shared/publications_sync.json`

Paste the shared Google Sheet URL there:

```json
{
  "source": "https://docs.google.com/spreadsheets/d/PASTE_ID_HERE/edit#gid=0"
}
```

The script can accept:

- a normal Google Sheets share URL
- a published Google Sheets CSV URL
- a local CSV path

If you paste a normal Google Sheets URL, the script will try to convert it to the CSV export endpoint automatically.

## Fallback local CSV

The CSV that feeds the sync script is:

`shared/publications_sheet.csv`

Keep it as a fallback template and as an example of the expected columns.

## Recommended workflow

1. Upload `shared/publications_sheet.csv` to Google Drive.
2. Open it with Google Sheets.
3. Share the sheet with collaborators.
4. Copy the shared sheet URL into:
   `shared/publications_sync.json`
5. Collaborators add or edit rows directly in Google Sheets.
6. Run:

```bash
python3 scripts/sync_publications.py
```

7. Rebuild the site with Docker and check `/publications/`.

## Optional one-off source override

If you want to test a different sheet once without touching the JSON config:

```bash
python3 scripts/sync_publications.py --source "PASTE_SHEET_URL_OR_CSV_URL_HERE"
```

## Minimal sheet columns

These are the only columns collaborators need:

- `status`
- `title`
- `doi`
- `authors`
- `year`
- `code`
- `website`
- `keywords`
- `selected`

Everything else is generated automatically when possible from DOI metadata.

## Editing rules for collaborators

- Use `publish` in `status` for rows that should go live.
- Use `preprint`, `draft`, or `ignore` to keep a row out of the published bibliography for now.
- Put author names in `authors` separated by semicolons:
  `Paolo Soda; Valerio Guarrasi`
- Use DOI when available. The script uses DOI first for metadata enrichment.
- Put keywords in `keywords` separated by semicolons:
  `medical imaging; explainability; multimodal learning`
- BibTeX keys are generated automatically by the script.
- `selected` accepts `true/false`, `yes/no`, or `1/0`.

## What `selected` means

`selected=true` marks a paper as featured.

In al-folio this is useful for:

- highlighting a subset of papers in future selected-paper sections
- distinguishing flagship papers from the full bibliography

If you do not care about featured papers right now, set it to `false` by default.

## What the script does

- reads the sheet
- enriches rows from Crossref when possible
- deduplicates by DOI, then by normalized title
- generates a BibTeX key automatically
- rewrites `_bibliography/papers.bib`

## Commands

Normal run:

```bash
python3 scripts/sync_publications.py
```

Run without metadata enrichment:

```bash
python3 scripts/sync_publications.py --no-enrich
```

## Important note

`_bibliography/papers.bib` becomes generated output for this workflow. Manual edits in that file will be overwritten the next time the sync script runs.
