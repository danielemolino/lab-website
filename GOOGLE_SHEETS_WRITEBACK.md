## Google Sheets Writeback

This optional workflow lets you push the local publications CSV directly into the shared Google Sheet.

### Files

- local source CSV: [shared/publications_sheet.csv](/mnt/c/Users/danie/OneDrive/Desktop/Codice/lab-website/shared/publications_sheet.csv)
- shared sheet link config: [shared/publications_sync.json](/mnt/c/Users/danie/OneDrive/Desktop/Codice/lab-website/shared/publications_sync.json)
- push script: [scripts/push_publications_to_sheet.py](/mnt/c/Users/danie/OneDrive/Desktop/Codice/lab-website/scripts/push_publications_to_sheet.py)

### Credentials

Do **not** commit credentials.

Expected local path:

`shared/google-service-account.json`

This file is ignored by git.

### Setup

1. Create a Google Cloud project.
2. Enable `Google Sheets API`.
3. Create a service account.
4. Create a JSON key for that service account.
5. Download the JSON key and place it at:
   `shared/google-service-account.json`
6. Share the target Google Sheet with the service account email as `Editor`.

### Install dependencies

```bash
python3 -m pip install google-api-python-client google-auth
```

### Push local CSV to the shared sheet

```bash
python3 scripts/push_publications_to_sheet.py
```

### Notes

- The script reads the spreadsheet URL from `shared/publications_sync.json`.
- It resolves the target tab using `gid`.
- It clears the target tab and rewrites it from the local CSV.
- This workflow is isolated and can be removed without affecting the site runtime.
