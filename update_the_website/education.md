# Aggiornare Education

Sheet: https://docs.google.com/spreadsheets/d/1euvx_n7_ZkgMWNYC88kvdDpcelphDvA2Oz1gQPuapGE/edit?gid=2062629998#gid=2062629998

File locale sincronizzato: `shared/education_sheet.csv`

Output generato:
- `_data/education_courses.yml`

## Regola generale

Il Google Sheet e `shared/education_sheet.csv` sono la sorgente dati. Non modificare a mano `_data/education_courses.yml`, perché viene rigenerato dallo script.

Per pubblicare un corso sul sito, mettere `publish` nella colonna `status`. Per tenerlo nello Sheet ma non mostrarlo, usare `draft`, `ignore` o `skip`.

## Campi dello Sheet

| Campo | Obbligatorio | Come compilarlo |
| --- | --- | --- |
| `status` | si | `publish` per mostrare il corso; `draft`, `ignore` o `skip` per escluderlo. |
| `title` | si | Nome del corso o attività didattica. |
| `degree_program` | si | Corso di laurea o sezione, es. `BSc in Industrial Engineering`, `MSc in Biomedical Engineering`, `Masters`, `Summer Schools`. |
| `cfu` | consigliato | Numero CFU, se disponibile. Lasciare vuoto se non applicabile. |
| `holder` | si | Titolare/i del corso. Per più persone usare `;`. |
| `assistants` | opzionale | Assistenti o docenti di supporto. Per più persone usare `;`. |

## Ordinamento sul sito

Lo script ordina i corsi per `degree_program` e poi per titolo. Le sezioni `Masters` e `Summer Schools` vengono spostate dopo i corsi di laurea.

## Comandi utili

Scaricare dati dal Google Sheet e rigenerare la pagina Education:

```bash
python3 scripts/sync_education.py
```

Caricare il CSV locale sul Google Sheet:

```bash
python3 scripts/push_education_to_sheet.py
```

