# Aggiornare Projects

Sheet: https://docs.google.com/spreadsheets/d/12G9q2Lj3wvY4BlBiDTBGtrbeQOH-1m73ZzK29rAMDbE/edit?gid=1869796220#gid=1869796220

Cartella immagini Drive: https://drive.google.com/drive/u/0/folders/1FAgonswyZ-6OBDGokE-mjd1RWATmDXZj

File locale sincronizzato: `shared/projects_sheet.csv`

Cartella locale immagini da caricare/aggiornare: `shared/project_images_uploads/`

Output generati:
- `_data/projects.yml`
- `_projects/<slug>.md`
- `assets/projects/<nome_file>`

## Regola generale

Il Google Sheet e `shared/projects_sheet.csv` sono la sorgente dati per il portfolio progetti. Non modificare a mano `_data/projects.yml` o le schede in `_projects/`, perché vengono rigenerate dallo script.

Per pubblicare un progetto sul sito, mettere `publish` nella colonna `status`. Per tenerlo nello Sheet ma non mostrarlo, usare `draft`, `ignore` o `skip`.

## Campi dello Sheet

| Campo | Obbligatorio | Come compilarlo |
| --- | --- | --- |
| `status` | si | `publish` per mostrare il progetto; `draft`, `ignore` o `skip` per escluderlo. |
| `name` | si | Nome sintetico usato nella card e nella pagina, es. `AIDA`, `PICTURE`, `XGeM+`. |
| `slug` | si | Nome tecnico della pagina, senza spazi, es. `aida-prin-2022`. Genera `/projects/<slug>/`. |
| `full_title` | si | Titolo completo mostrato sotto al nome sintetico. |
| `project_state` | si | `active` oppure `ended`. Serve per separare Active Projects e Completed Projects. |
| `project_type` | consigliato | Tipo mostrato nella hero della scheda progetto, es. `Clinical Application`, `Technology Transfer`. |
| `img` | si | Nome file immagine nella cartella Drive, es. `aida-visual.jpg`. Lo script lo copia/scarica e nel sito genera `/assets/projects/aida-visual.jpg`. |
| `collaborators` | consigliato | Collaboratori separati da `;`. |
| `start_year` | consigliato | Anno di inizio, solo anno, es. `2023`. |
| `end_year` | consigliato | Anno di fine, solo anno, es. `2026`. |
| `overview` | si | Testo della sezione Overview. Per più paragrafi usare `||` tra un paragrafo e l'altro. |
| `grant_number` | opzionale | Grant number o riferimento bando, se disponibile. |
| `research_directions` | si | Punti chiave separati da `;`. |
| `project_filter` | si | Slug usato nelle colonne `project_1`, `project_2`, `project_3` delle pubblicazioni. Deve essere unico. |
| `official_page` | opzionale | URL della pagina ufficiale del progetto. Se vuoto, il bottone non compare. |
| `description` | consigliato | Descrizione breve nella hero della scheda progetto. |
| `focus_areas` | consigliato | Tag della hero separati da `;`. |

## Project filter

`project_filter` è il collegamento tra progetti e pubblicazioni. Lo stesso valore va usato nelle colonne `project_1`, `project_2`, `project_3` dello Sheet Publications.

Il dropdown dei progetti in Publications viene aggiornato da `scripts/push_publications_to_sheet.py` leggendo `shared/projects_sheet.csv`. Quindi, quando si aggiunge o rinomina un progetto, bisogna aggiornare Projects prima di aggiornare Publications.

## Immagini progetto

Il sito è statico, quindi le immagini devono finire nel repository in `assets/projects/`. La cartella Drive condivisa è l'area di upload per chi aggiorna i contenuti.

Workflow consigliato:

1. Caricare l'immagine nella cartella Drive condivisa dei progetti.
2. Inserire nel campo `img` solo il nome file, ad esempio:

```text
aida-visual.jpg
```

3. Eseguire lo sync. Lo script prova a scaricare la cartella Drive in `shared/project_images_uploads/` e copia i file in `assets/projects/`.

Se il download Drive non riesce per problemi di rete, si può scaricare manualmente il file nella cartella `shared/project_images_uploads/` e rilanciare lo sync.

Usare nomi file brevi e stabili, senza spazi:

```text
aida-visual.jpg
picture-overview.png
xgem-model.png
```

## Comandi utili

Creare o aggiornare il CSV partendo dalle schede progetto già presenti:

```bash
python3 scripts/sync_projects.py --bootstrap-from-pages
```

Scaricare dati dal Google Sheet e rigenerare il sito:

```bash
python3 scripts/sync_projects.py
```

Usare il CSV locale senza scaricare lo Sheet:

```bash
python3 scripts/sync_projects.py --source shared/projects_sheet.csv
```

Caricare il CSV locale sul Google Sheet Projects:

```bash
python3 scripts/push_projects_to_sheet.py
```

Dopo aver aggiornato Projects, riapplicare i dropdown Publications:

```bash
python3 scripts/push_publications_to_sheet.py --helpers-only
```
