# Team Sheet Sync

This workflow lets colleagues maintain team profiles in a shared spreadsheet and regenerate:

- `_data/team.yml`
- `_pages/team/<slug>.md`
- `assets/team_photos/*`

## Files in the repo

- Team sheet template: `shared/team_sheet.csv`
- Team sync config: `shared/team_sync.json`
- Sync script: `scripts/sync_team.py`

## Shared Google Sheet

1. Upload `shared/team_sheet.csv` to Google Drive.
2. Open it with Google Sheets.
3. Share it with collaborators.
4. Paste the shared sheet URL into:

`shared/team_sync.json`

Example:

```json
{
  "source": "https://docs.google.com/spreadsheets/d/PASTE_ID_HERE/edit?gid=0#gid=0",
  "photos_source": "shared/team_photos_uploads",
  "photos_drive_folder": "https://drive.google.com/drive/folders/PASTE_FOLDER_ID_HERE"
}
```

The script accepts:

- a normal Google Sheets share URL
- a published Google Sheets CSV URL
- a local CSV path

## Team photo folder

For photos, the reliable base workflow is local-folder based.

Use this folder in the repo:

`shared/team_photos_uploads`

Every collaborator photo should be saved there with this naming convention:

- `name_surname.png`
- `name_surname.jpg`
- `name_surname.jpeg`
- `name_surname.webp`

Examples:

- `paolo_soda.png`
- `valerio_guarrasi.jpg`

The script copies matched photos into:

`assets/team_photos`

and uses those site-ready paths automatically in `team.yml`.

If no matching photo is found, the script uses the default placeholder image.

## Recommended photo workflow

The easiest setup is:

1. Create a shared Drive folder for headshots.
2. Ask everyone to upload one portrait there with the exact `name_surname.ext` format.
3. Sync or download that shared folder locally into:
   `shared/team_photos_uploads`
4. Run the sync script.

This avoids fragile Google Drive scraping and keeps the process predictable.

If the Drive folder is public, the script can also try a best-effort automatic
download before matching files locally. Set the folder URL in:

`shared/team_sync.json`

The automatic downloader expects:

- the folder to be readable with the link
- standard names like `name_surname.png`
- common image extensions only

## Minimal columns

- `status`
- `name`
- `surname`
- `role`
- `title`
- `email`
- `external_url`
- `scholar_url`
- `orcid_url`
- `interests`
- `short_bio`
- `bio`
- `github_url`
- `linkedin_url`

## Field notes

- `role` should be one of:
  - `pi`
  - `faculty`
  - `researcher`
  - `postdoc`
  - `phd`
  - `student`
  - `collaborator`
  - `alumni`
- `interests` should use semicolons:
  `medical imaging; multimodal learning; clinical AI`
- `slug` is generated automatically.
- Team ordering is generated automatically:
  first by role group, then alphabetically by surname.

## Selected publications

The script also reads `_bibliography/papers.bib` and automatically attaches selected publications to each member profile.

Current rule:

- only papers with `selected=true`
- author match based on the member name and surname

This is enough for the current workflow and removes the need to hand-maintain publication lists in member pages.

## Commands

Normal run:

```bash
python3 scripts/sync_team.py
```

Override the source once:

```bash
python3 scripts/sync_team.py --source "PASTE_SHEET_URL_OR_CSV_URL_HERE"
```

Override the local photos folder once:

```bash
python3 scripts/sync_team.py --photos-source "PATH/TO/LOCAL/PHOTO/FOLDER"
```

Override the Google Drive photo folder once:

```bash
python3 scripts/sync_team.py --photos-drive-folder "PASTE_PUBLIC_DRIVE_FOLDER_URL_HERE"
```

## Important note

`_data/team.yml` and `_pages/team/*.md` become generated output for this workflow. Manual edits in those files will be overwritten the next time the sync script runs.
