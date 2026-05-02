# Social Media Use and Loneliness in Europe: Does Age Matter?

> Progetto del corso **Human Data Science** — Università di Bologna, A.A. 2025–2026  
> Gruppo 2: Matteo Boscherini · Alessandro Campedelli · Alessio Manieri

---

## Descrizione

Questo repository contiene il codice Python utilizzato per analizzare l'associazione tra intensità d'uso dei social media e solitudine percepita tra gli utenti adulti europei, con focus sul ruolo moderatore dell'età.

Il dataset di riferimento è l'**EU Loneliness Survey 2022** (EU-LS 2022), prodotto dal Joint Research Centre (JRC) della Commissione Europea, che include 25.646 osservazioni distribuite su 27 paesi UE.

---

## Struttura del repository

```
eu-loneliness-socialmedia/
│
├── eu-loneliness-socialmedia.ipynb        # Notebook principale con tutto il codice
├── eu_loneliness_survey_eu27_values.csv   # Dataset grezzo EU-LS 2022
│
├── pyproject.toml                         # Dipendenze del progetto (gestite con uv)
├── uv.lock                                # Lock file delle dipendenze
├── .python-version                        # Versione Python usata dal progetto
│
├── LICENSE
├── .gitignore
├── Studio_Gruppo_2_HDS.pdf                # Relazione Finale dello studio
│
└── output/                                # Generata automaticamente all'esecuzione
    ├── dataset/
    │   └── eu_ls_clean.csv                # Dataset pulito (Step 1)
    └── figures/                           # Figure generate dal notebook
        ├── step1_diagnostici.png
        ├── step2_eda.png
        ├── step3_ols.png
        ├── step3_nonlinearita_giovani.png
        └── step4_rf_pdp.png
```

> **Nota:** la cartella `output/` viene creata automaticamente all'esecuzione del notebook. Non è necessario crearla manualmente.

---

## Dataset

Il dataset grezzo **EU Loneliness Survey 2022** è incluso nel repository (`eu_loneliness_survey_eu27_values.csv`).

È disponibile anche per download separato dalla repository ufficiale del JRC:

📥 **Download dataset:**  
[EU Loneliness Survey — Dataset EU27](https://jeodpp.jrc.ec.europa.eu/ftp/jrc-opendata/EU-Loneliness-Survey/Data%20EU27%20sample.zip)
---

## Requisiti

Il progetto utilizza **[uv](https://docs.astral.sh/uv/)** come package manager. Le dipendenze e la versione di Python sono definite in `pyproject.toml` e bloccate in `uv.lock`.

### Installare uv (se non già presente)

**macOS / Linux:**
```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
```

**Windows:**
```powershell
powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"
```

### Librerie principali

| Libreria | Utilizzo |
|---|---|
| `pandas` | Manipolazione e pulizia dei dati |
| `numpy` | Operazioni numeriche |
| `matplotlib` | Visualizzazioni |
| `seaborn` | Visualizzazioni statistiche |
| `scipy` | Correlazione di Spearman, test statistici |
| `statsmodels` | Regressione OLS con errori robusti HC3 |
| `scikit-learn` | Random Forest, permutation importance, PDP |
| `jupyter` | Esecuzione del notebook |

---

## Come eseguire il codice

### 1. Clonare il repository

```bash
git clone https://github.com/alessandrocampedelli/eu-loneliness-socialmedia.git
cd eu-loneliness-socialmedia
```

### 2. Installare le dipendenze

```bash
uv sync
```

`uv` leggerà `pyproject.toml` e `uv.lock`, creerà automaticamente un ambiente virtuale e installerà tutte le dipendenze nella versione esatta usata durante lo sviluppo.

### 3. Avviare Jupyter

```bash
uv run jupyter notebook eu-loneliness-socialmedia.ipynb
```

### 4. Eseguire le celle in ordine

Il notebook è strutturato in step sequenziali. **È necessario eseguire le celle nell'ordine corretto**, poiché ogni step utilizza l'output del precedente:

| Step | Contenuto | Output generato |
|---|---|---|
| **Step 1** | Caricamento, pulizia, preparazione variabili | `output/dataset/eu_ls_clean.csv` |
| **Step 2** | Analisi esplorativa (EDA), correlazioni Pearson e Spearman | Figure EDA |
| **Step 3** | Regressione OLS lineare per fascia d'età, F-test interazione, analisi non-linearità 16–34 | Figure OLS + forest plot |
| **Step 4** | Random Forest, permutation importance, Partial Dependence Plot | Figure PDP |

Per eseguire tutto il notebook in un colpo solo: **Kernel → Restart & Run All**.

---

## Descrizione delle variabili principali

| Variabile | Tipo | Descrizione |
|---|---|---|
| `score_UCLA` | Numerica (3–9) | Variabile dipendente: somma dei 3 item UCLA sulla solitudine percepita |
| `intensita_sm` | Ordinale (1–8) | Variabile indipendente: tempo giornaliero sui social media |
| `fascia_eta` | Categoriale | Fascia d'età: `16–34`, `35–54`, `55+` |
| `sesso` | Binaria | Genere del rispondente |
| `education` | Ordinale (ISCED) | Livello di istruzione |
| `income` | Ordinale (1–10) | Decile di reddito nella distribuzione nazionale |
| `paese` | Categoriale (1–27) | Paese UE (usato come effetto fisso nei modelli) |

---

## Principali risultati

- La correlazione tra intensità d'uso dei social media e solitudine percepita è **positiva e significativa** in tutte le fasce d'età (p < 0.001).
- L'effetto è massimo nella fascia **35–54** (β = +0.126) e sostanzialmente equivalente nella **16–34** (β = +0.114); si attenua nella **55+** (β = +0.075).
- Nella fascia **16–34** si osserva un'accelerazione non lineare concentrata negli ultimi livelli di uso (6–8), confermata indipendentemente dal PDP del Random Forest.
- OLS e Random Forest convergono sulla stessa narrativa su direzione, forma e gradiente per età.

---

## Autori

| Nome | Matricola | Email
|---|---|---|
| Matteo Boscherini | 0001239423 | matteo.boscherini@studio.unibo.it
| Alessandro Campedelli | 0001242904 | alessandr.campedell3@studio.unibo.it
| Alessio Manieri | 0001237538 | alessio.manieri@studio.unibo.it