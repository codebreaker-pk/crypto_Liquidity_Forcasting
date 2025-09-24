
# Pipeline & Architecture

> End-to-end blueprint for the Cryptocurrency Liquidity Prediction project:
> how data flows, how features/models are produced, and how the Flask app serves predictions.

## Repository Layout
```
crypto_liquidity_project/
├── data/
│   ├── raw/
│   └── processed/
│       ├── merged_coin_gecko.csv
│       └── engineered_features_lag.csv
├── notebooks/
│   ├── data_preparation.ipynb
│   ├── eda.ipynb
│   ├── feature_engineering.ipynb
│   ├── modeling.ipynb
│   └── evaluation_testing.ipynb
├── artifacts/
│   ├── models/
│   │   └── RidgeCV_logtarget.joblib
│   └── metrics/
│       └── results.csv
├── templates/index.html
├── static/style.css
├── app.py
├── docs/
│   ├── HLD.md
│   ├── LLD.md
│   └── PIPELINE_ARCHITECTURE.md
└── reports/
    ├── EDA_REPORT.md
    └── FINAL_REPORT.md
```
## Dataflow
```mermaid
flowchart LR
    A["Raw CSVs (data/raw)"] --> B["Preprocess & Merge"]
    B --> C["Feature Engineering"]
    C --> D["Modeling & Selection"]
    D --> E["Best Model Export (.joblib)"]
    C --> F["Evaluation & Plots"]
    E --> G["Flask Inference (app.py)"]
    G --> H["UI (templates + static)"]
```

## Orchestration (sequence)
```mermaid
sequenceDiagram
  participant NB as Notebooks
  participant DS as data/processed
  participant AR as artifacts
  participant SV as Flask
  NB->>DS: merged_coin_gecko.csv
  NB->>DS: engineered_features_lag.csv
  NB->>AR: results.csv + RidgeCV_logtarget.joblib
  SV->>AR: Load model
  SV->>SV: Derive features → predict (expm1)
  SV-->>User: Liquidity ratio + label
```

## Run Commands
```bash
python -m venv .venv
.venv\Scripts\activate
pip install -r requirements.txt
python app.py
```
