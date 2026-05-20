# Aggiornare il sito

Questa cartella contiene le istruzioni operative per aggiornare i contenuti sincronizzati del sito ArCo Lab.

## File disponibili

| File | Cosa spiega | Sheet / sorgente |
| --- | --- | --- |
| `team.md` | Persone, ruoli, foto profilo, profili membro. | `shared/team_sheet.csv` |
| `projects.md` | Portfolio progetti, schede progetto, immagini, filtri progetto. | `shared/projects_sheet.csv` |
| `publications.md` | Pubblicazioni, keyword, collegamento ai progetti, BibTeX/YAML. | `shared/publications_sheet.csv` |
| `news.md` | News copiate da LinkedIn, testo editoriale e link al post. | `shared/news_sheet.csv` |
| `education.md` | Corsi e attività didattiche. | `shared/education_sheet.csv` |

## Regola principale

I file generati dal sync non vanno modificati a mano. Le sorgenti sono sempre:

- Google Sheet condiviso;
- CSV locale in `shared/`;
- cartelle upload locali/Drive per immagini e foto.

I file generati includono `_data/*.yml`, `_projects/*.md`, `_pages/team/*.md`, `_bibliography/papers.bib` e asset copiati in `assets/`.

## Sync e Push

I comandi `sync_*` scaricano o leggono il CSV sorgente e rigenerano il sito:

```bash
python3 scripts/sync_team.py
python3 scripts/sync_projects.py
python3 scripts/sync_publications.py --no-enrich
python3 scripts/sync_news.py
python3 scripts/sync_education.py
```

I comandi `push_*` caricano il CSV locale sul Google Sheet e riapplicano dropdown/validazioni quando previste:

```bash
python3 scripts/push_team_to_sheet.py
python3 scripts/push_projects_to_sheet.py
python3 scripts/push_publications_to_sheet.py
python3 scripts/push_news_to_sheet.py
python3 scripts/push_education_to_sheet.py
```

Per aggiornare solo i tab di supporto e i dropdown di Publications, senza riscrivere le pubblicazioni:

```bash
python3 scripts/push_publications_to_sheet.py --helpers-only
```

Ordine consigliato quando si aggiornano più sezioni:

1. Projects, se sono stati aggiunti o rinominati progetti.
2. Publications, perché i dropdown `project_1`, `project_2`, `project_3` dipendono dal tracking Projects.
3. Team, News, Education quando necessario.

## Config locali

I file `shared/*_sync.json` contengono i link dei Google Sheet e restano locali/ignorati da git quando contengono dati reali. I file `shared/*_sync.example.json` sono template condivisibili.

Per i comandi `push_*` serve il service account locale:

```text
shared/google-service-account.json
```

Ogni Google Sheet deve essere condiviso come Editor con l'email `client_email` presente in quel JSON.

## Prima di pubblicare

Dopo modifiche ai contenuti, eseguire almeno il relativo comando `sync_*`. Se sono cambiati progetti o pubblicazioni, aggiornare anche i dropdown Publications.

Quando l'ambiente lo permette, verificare il sito con la build Jekyll/Docker prevista dal repository.
