# Aggiornare Publications

Sheet: https://docs.google.com/spreadsheets/d/1iOZfd7Cqw7oQoabZ7ejj5Xajre2b8uhXbtp8x7mR5K0/edit?gid=1832105955#gid=1832105955

File locale sincronizzato: `shared/publications_sheet.csv`

Output generati:
- `_bibliography/papers.bib`
- `_data/publications.yml`

## Regola generale

Il Google Sheet e `shared/publications_sheet.csv` sono la sorgente dati. Non modificare a mano `_bibliography/papers.bib` o `_data/publications.yml`, perché vengono rigenerati dallo script.

Per pubblicare una riga sul sito, mettere `publish` nella colonna `status`. Per tenerla nello Sheet ma non mostrarla, usare `draft`, `ignore` o `skip`.

## Campi dello Sheet

| Campo | Obbligatorio | Come compilarlo |
| --- | --- | --- |
| `status` | si | `publish` per mostrare la pubblicazione; `draft`, `ignore` o `skip` per escluderla. |
| `title` | si | Titolo completo della pubblicazione. |
| `doi` | consigliato | DOI senza `https://doi.org/`, es. `10.1016/j.compmedimag.2026.102718`. Per arXiv usare DOI arXiv se disponibile, es. `10.48550/arxiv...`. |
| `authors` | si | Autori separati da `;`, es. `Paolo Soda; Valerio Guarrasi`. |
| `year` | si | Anno a 4 cifre. |
| `code` | opzionale | URL GitHub o repository codice. |
| `website` | opzionale | URL pagina progetto, demo, dataset o paper website. |
| `keywords` | consigliato | Una o più keyword controllate, separate da `;`. |
| `project_1` | consigliato quando applicabile | Primo `project_filter` associato alla pubblicazione. Usare il dropdown. |
| `project_2` | opzionale | Secondo `project_filter`, se la pubblicazione appartiene a più progetti. |
| `project_3` | opzionale | Terzo `project_filter`, se necessario. |

La colonna `selected` è stata rimossa: non serve più al flusso attuale del sito. Le pagine Team mostrano automaticamente le pubblicazioni più recenti, non una selezione manuale.

## Keyword ammesse

Usare solo questi valori in `keywords`:

```text
medical imaging
multimodal learning
generative AI
foundation models
clinical prediction
explainability
radiomics
oncology
COVID-19
industrial AI
```

Per più keyword nella stessa cella:

```text
medical imaging; generative AI; oncology
```

## Progetti ammessi

Usare solo i valori `project_filter` presenti nel tracking Projects (`shared/projects_sheet.csv` / Google Sheet Projects). I progetti disponibili nei dropdown Publications vengono derivati da lì, così l'elenco non va mantenuto a mano in due punti diversi.

Per associare più progetti alla stessa pubblicazione, usare una colonna per progetto:

```text
project_1 = aida
project_2 = picture
project_3 =
```

Non inserire più progetti nella stessa cella.

## Dropdown keyword e progetti

Lo script `scripts/push_publications_to_sheet.py` aggiorna anche due tab di supporto:

- `keyword_vocab`
- `project_ids`

e applica una validazione sulle colonne `keywords`, `project_1`, `project_2` e `project_3`. Il tab `project_ids` viene generato dal tracking Projects.

Nota pratica: Google Sheets non gestisce in modo affidabile un vero multi-select nella stessa cella via API. Per questo i progetti sono divisi in più colonne, ognuna con il proprio dropdown.

## Comandi utili

Scaricare dati dal Google Sheet e rigenerare BibTeX/YAML:

```bash
python3 scripts/sync_publications.py --no-enrich
```

Usare `--no-enrich` per evitare attese o errori temporanei da Crossref/OpenAlex.

Caricare il CSV locale sul Google Sheet, aggiornare tab di supporto e dropdown:

```bash
python3 scripts/push_publications_to_sheet.py
```

Usare il CSV locale e rigenerare soltanto i file del sito, senza scaricare lo Sheet:

```bash
python3 scripts/sync_publications.py --source shared/publications_sheet.csv --no-enrich
```

Se sono stati aggiunti o modificati progetti, eseguire prima:

```bash
python3 scripts/sync_projects.py
```

Per aggiornare solo i tab di supporto e i dropdown, senza riscrivere le righe dello Sheet Publications:

```bash
python3 scripts/push_publications_to_sheet.py --helpers-only
```
