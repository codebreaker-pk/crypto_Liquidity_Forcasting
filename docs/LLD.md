
# Low-Level Design (LLD)

## Data Structures
- Input (UI):
  - price, 1h, 24h, 7d, 24h_volume, mkt_cap
  - Optional prev-day: price_prev, vol24h_prev, mcap_prev
- Engineered (server-side):
  - price_ma_3/5 (fallback = price)
  - vol_3d (fallback = 0)
  - lag1: liquidity_ratio_lag1 (= vol_prev/mcap_prev), price_lag1, 24h_volume_lag1, mkt_cap_lag1
  - returns: price_ret_1d, vol_chg_1d, mcap_chg_1d (0 if prev missing)
  - logs: log_price, log_vol, log_mcap

## Functions
- `build_feature_row(form) -> np.ndarray(1x19)`
  - Parses form; computes engineered features with safe defaults.
- `interpret_liquidity(lr) -> (label, color, tip)`
  - Thresholds:
    - `<0.02` Very Low
    - `0.02–0.06` Low
    - `0.06–0.15` Moderate
    - `0.15–0.40` High
    - `>0.40` Very High
- Model I/O
  - Train on `log1p(y)`; infer `pred = expm1(model.predict(X))`

## Training (notebooks)
- `data_preparation.ipynb` → merge, clean, label
- `eda.ipynb` → visuals & stats
- `feature_engineering.ipynb` → MAs, lags, returns, logs, save CSV
- `modeling.ipynb` → fit/evaluate models; save `artifacts/models/*.joblib`
- `evaluation_testing.ipynb` → full-matrix eval & plots

## Flask Endpoints
- `GET /` → render form
- `POST /predict` → build features → predict → render result

## Files
- `app.py` (Flask server)
- `templates/index.html` (Jinja2 UI)
- `static/style.css` (modern glass UI)
- `artifacts/models/RidgeCV_logtarget.joblib` (saved model)
