# Education Sheet Sync

This workflow lets the lab manage teaching entries from a shared spreadsheet without creating a separate page for each course.

## Files

- shared sheet config: `shared/education_sync.json`
- local fallback CSV: `shared/education_sheet.csv`
- generated site data: `_data/education_courses.yml`
- sync script: `scripts/sync_education.py`
- push script: `scripts/push_education_to_sheet.py`

## Sheet columns

Use these columns:

- `status`
- `title`
- `degree_program`
- `cfu`
- `description`
- `holder`
- `assistants`
- `year`
- `term`

`assistants` should be separated by semicolons:

```text
Assistant One; Assistant Two
```

## Workflow

1. Upload `shared/education_sheet.csv` to Google Drive.
2. Open it with Google Sheets.
3. Share it with collaborators.
4. Paste the shared sheet URL into `shared/education_sync.json`.
5. Run:

```bash
python3 scripts/sync_education.py
```

This updates `_data/education_courses.yml` and mirrors the remote CSV back into `shared/education_sheet.csv`.

If you edit the local CSV first and want to push it to the shared sheet:

```bash
python3 scripts/push_education_to_sheet.py
```
