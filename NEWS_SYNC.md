# News Sheet Sync

This workflow keeps the site news page lightweight by mirroring LinkedIn-style updates from a shared spreadsheet.

## Files

- shared sheet config: `shared/news_sync.json`
- local fallback CSV: `shared/news_sheet.csv`
- generated site data: `_data/news_feed.yml`
- sync script: `scripts/sync_news.py`
- push script: `scripts/push_news_to_sheet.py`

## Sheet columns

- `status`
- `date`
- `title`
- `summary`
- `category`
- `linkedin_url`
- `image_url` (optional)

## Workflow

1. Upload `shared/news_sheet.csv` to Google Drive.
2. Open it with Google Sheets.
3. Share it with collaborators.
4. Paste the shared sheet URL into `shared/news_sync.json`.
5. Run:

```bash
python3 scripts/sync_news.py
```

This updates `_data/news_feed.yml` and mirrors the remote CSV back into `shared/news_sheet.csv`.

To push the local CSV into the shared sheet:

```bash
python3 scripts/push_news_to_sheet.py
```
