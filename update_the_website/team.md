# Aggiornare Team

Sheet: https://docs.google.com/spreadsheets/d/1dS6CUhLJl63J7-HA_Mg-IhH2OhcY3e_CGxWou26HmPk/edit?gid=1005841770#gid=1005841770

Cartella foto Drive: https://drive.google.com/drive/u/0/folders/1y4jPyGO_wP5mk0rXEeSzzJ2Tbh0O2zsX

File locale sincronizzato: `shared/team_sheet.csv`

Output generati:
- `_data/team.yml`
- `_pages/team/<slug>.md`
- `assets/team_photos/<nome_file_normalizzato>`

## Regola generale

Il Google Sheet e `shared/team_sheet.csv` sono la sorgente dati. Non modificare a mano `_data/team.yml` o le pagine in `_pages/team/`, perché vengono rigenerate dallo script.

Per pubblicare una persona sul sito, mettere `publish` nella colonna `status`. Per tenerla nello Sheet ma non mostrarla, usare `draft`, `ignore` o `skip`.

## Campi dello Sheet

| Campo | Obbligatorio | Come compilarlo |
| --- | --- | --- |
| `status` | si | `publish` per mostrare la persona; `draft`, `ignore` o `skip` per escluderla. |
| `name` | si | Nome, es. `Paolo`. |
| `surname` | si | Cognome, es. `Soda`. |
| `role` | si | Valore controllato: `pi`, `faculty`, `researcher`, `postdoc`, `phd`. |
| `title` | si | Titolo/posizione completa mostrata nel profilo, es. `Full Professor of Information Processing Systems`. |
| `email` | consigliato | Email istituzionale o vuoto. |
| `scholar_url` | consigliato | Profilo Google Scholar. Serve anche per associare meglio le pubblicazioni. |
| `orcid_url` | opzionale | Profilo ORCID. |
| `interests` | consigliato | Lista separata da `;`, es. `Medical Imaging; Generative AI; Radiomics`. |
| `bio` | consigliato | Breve biografia in inglese, 2-5 frasi. |
| `github_url` | opzionale | URL GitHub personale. |
| `linkedin_url` | opzionale | URL LinkedIn personale. |

## Foto profilo

Le foto vanno caricate in `shared/team_photos_uploads/` oppure nella cartella Drive indicata sopra.

Il nome file deve seguire il formato:

```text
nome_cognome.jpg
nome_cognome.png
nome_cognome.jpeg
nome_cognome.webp
```

Esempi:

```text
paolo_soda.png
valerio_guarrasi.png
camillo_maria_caruso.jpg
```

Lo script normalizza nome e cognome, copia la foto in `assets/team_photos/` e la collega automaticamente. Se non trova la foto, usa il placeholder.

## Pubblicazioni nei profili

Ogni scheda membro mostra automaticamente fino alle 10 pubblicazioni più recenti associate al nome della persona nella bibliografia.

Il bottone `Browse All Publications` apre la pagina Publications filtrata per quel membro, quindi permette di vedere l'elenco completo oltre alle 10 mostrate nel profilo.

## Dropdown ruolo

Lo script `scripts/push_team_to_sheet.py` applica una validazione sul campo `role` del Google Sheet. Il menu deve contenere solo:

```text
pi
faculty
researcher
postdoc
phd
```

## Comandi utili

Scaricare dati dal Google Sheet e rigenerare il sito:

```bash
python3 scripts/sync_team.py
```

Usare il CSV locale senza scaricare lo Sheet:

```bash
python3 scripts/sync_team.py --source shared/team_sheet.csv
```

Caricare il CSV locale sul Google Sheet e riapplicare il dropdown:

```bash
python3 scripts/push_team_to_sheet.py
```
