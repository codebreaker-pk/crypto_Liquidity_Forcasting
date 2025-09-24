# High-Level Design (HLD) — Crypto Liquidity Prediction

## Goal
Predict short-term **liquidity ratio** to flag thin-liquidity regimes and aid risk management.

## Key Concept
**Liquidity Ratio** = `24h_volume / mkt_cap`  
Higher → easier execution (lower slippage).

## System Overview
- **Data Layer**
  - Raw CSVs (CoinGecko daily) → `data/raw/`
  - Cleaned & merged → `data/processed/merged_coin_gecko.csv`
  - Feature engineered + lags → `data/processed/engineered_features_lag.csv`
- **ML Layer**
  - Target: `liquidity_ratio` (modeled as `log1p(y)`)
  - Models tried: Linear, RidgeCV, LassoCV, RandomForest, XGB, LGBM, HistGB
  - Best: **RidgeCV with scaling** (stable, strong accuracy)
- **Serving Layer**
  - **Flask** app (`app.py`)
  - Minimal UI (HTML/CSS) → user inputs 6 core fields
  - Backend derives features; model predicts; UI shows label + guidance

## Data Flow (Pipeline)
```mermaid
flowchart LR
A[Raw CSVs] --> B[Preprocess & Merge]
B --> C[Feature Engineering\n(MAs, lags, returns, logs)]
C --> D[Train/Test Split]
D --> E[Model Training & Eval\n(log target)]
E --> F[Best Model Export\njoblib]
F --> G[Flask App Inference]

```
## Non-Functional

- Reproducible notebooks & fixed directory layout

- Lightweight runtime (RidgeCV); fast inference

- Extensible to more dates & external signals (order-book, social)

## Risks & Mitigation

- Short history → add more dates; validate time-series-wise

- Proxy label → enrich with spread/depth when available

- Outliers → log transforms, winsorization if needed