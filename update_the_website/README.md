# Aggiornare il sito

Questa cartella contiene le istruzioni operative per aggiornare i contenuti sincronizzati del sito ArCo Lab.

## File disponibili

| File | Cosa spiega | Sheet / sorgente |
| --- | --- | --- |
| `team.md` | Persone, ruoli, foto profilo, profili membro. | `shared/team_sheet.csv` |
| `projects.md` | Portfolio progetti, schede progetto, immagini, filtri progetto. | `shared/projects_sheet.csv` |
| `publications.md` | Pubblicazioni, keyword, collegamento ai progetti, BibTeX/YAML. | `shared/publications_sheet.csv` |
| `news.md` | News copiate da LinkedIn, testo editoriale, link al post e immagini locali. | `shared/news_sheet.csv` |
| `education.md` | Corsi e attività didattiche. | `shared/education_sheet.csv` |

## Regola principale

I file generati dal sync non vanno modificati a mano. Le sorgenti sono sempre:

- Google Sheet condiviso;
- CSV locale in `shared/`;
- cartelle upload locali/Drive per immagini e foto.

I file generati includono `_data/*.yml`, `_projects/*.md`, `_pages/team/*.md`, `_bibliography/papers.bib` e asset copiati in `assets/`.

Le immagini usate dal sito devono essere statiche e committate nel repository. Gli script possono partire da URL o cartelle di upload, ma il risultato finale deve stare in `assets/`, per esempio:

- `assets/team/` per le foto profilo;
- `assets/projects/` per le immagini progetto;
- `assets/news/` per le immagini news.

## Sync e Push

I comandi `sync_*` scaricano o leggono il CSV sorgente e rigenerano il sito:

```bash
python scripts/sync_team.py
python scripts/sync_projects.py
python scripts/sync_publications.py --no-enrich
python scripts/sync_news.py
python scripts/sync_education.py
```

I comandi `push_*` caricano il CSV locale sul Google Sheet e riapplicano dropdown/validazioni quando previste:

```bash
python scripts/push_team_to_sheet.py
python scripts/push_projects_to_sheet.py
python scripts/push_publications_to_sheet.py
python scripts/push_news_to_sheet.py
python scripts/push_education_to_sheet.py
```

Per aggiornare solo i tab di supporto e i dropdown di Publications, senza riscrivere le pubblicazioni:

```bash
python scripts/push_publications_to_sheet.py --helpers-only
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

Controllare sempre i file generati prima del commit:

```bash
git status --short
git diff --check
```

Per una modifica alle news con immagini, il commit deve includere almeno:

```bash
git add _data/news_feed.yml assets/news
```

Se e' stata cambiata anche la pagina o il template news, includere anche:

```bash
git add _pages/news.md
```

Su GitHub Pages i path delle immagini locali devono passare da `relative_url` nei template Liquid, altrimenti possono funzionare in locale ma non nel sito pubblicato con `baseurl`.
