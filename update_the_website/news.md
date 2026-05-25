# Aggiornare News

Sheet: https://docs.google.com/spreadsheets/d/1XcMmFZr8Os7sye7R3X8LMB0q0T_EFcDmfXEeexqQvyE/edit?gid=617535761#gid=617535761

File locale sincronizzato: `shared/news_sheet.csv`

Cartella locale immagini da caricare/aggiornare: `shared/news_images_uploads/`

Output generato:
- `_data/news_feed.yml`
- `assets/news/<nome_file>`

## Regola generale

Il Google Sheet e `shared/news_sheet.csv` sono la sorgente dati. Non modificare a mano `_data/news_feed.yml`, perché viene rigenerato dallo script.

Per pubblicare una news sul sito, mettere `publish` nella colonna `status`. Per tenerla nello Sheet ma non mostrarla, usare `draft`, `ignore` o `skip`.

Le news sono una replica editoriale del feed LinkedIn: non è un embed automatico. Ogni riga deve contenere testo già pronto per il sito.

## Campi dello Sheet

| Campo | Obbligatorio | Come compilarlo |
| --- | --- | --- |
| `status` | si | `publish` per mostrare la news; `draft`, `ignore` o `skip` per escluderla. |
| `date` | si | Data in formato `YYYY-MM-DD`. Viene usata per ordinare le news. |
| `title` | si | Titolo breve della news. |
| `summary` | si | Testo della news in 1-3 frasi. Deve essere leggibile anche senza aprire LinkedIn. |
| `category` | consigliato | Categoria breve, es. `Publication`, `Talk`, `Milestone`, `Project`, `Award`. |
| `linkedin_url` | consigliato | URL del post LinkedIn originale. |
| `image_url` | opzionale | URL immagine o nome file. Lo script scarica/copia l'immagine e nel sito genera un path locale in `assets/news/`. Lasciare vuoto se non serve. |

## URL LinkedIn

La colonna `linkedin_url` deve contenere il link diretto al post LinkedIn, ad esempio:

```text
https://www.linkedin.com/feed/update/urn:li:activity:...
```

oppure:

```text
https://www.linkedin.com/posts/arco-unicampus_...
```

Il sito usa questo URL come link esterno verso il post originale. Il testo mostrato nel sito viene invece dalla colonna `summary`.

## Immagini

Se `image_url` contiene un URL diretto a un'immagine, lo script prova a scaricarla in `assets/news/` e poi usa il path locale nel sito.

Esempio valido:

```text
https://media.licdn.com/dms/image/...
```

Se si preferisce gestire il file manualmente:

1. salvare l'immagine in `shared/news_images_uploads/`;
2. inserire in `image_url` solo il nome file, ad esempio `isbi-pet-ct.jpg`;
3. eseguire lo sync.

Il file finale usato dal sito sarà:

```text
/assets/news/isbi-pet-ct.jpg
```

## Comandi utili

Scaricare dati dal Google Sheet e rigenerare il feed:

```bash
python3 scripts/sync_news.py
```

Usare il CSV locale senza scaricare lo Sheet:

```bash
python3 scripts/sync_news.py --source shared/news_sheet.csv
```

Caricare il CSV locale sul Google Sheet:

```bash
python3 scripts/push_news_to_sheet.py
```
