
---

# 5) `reports/FINAL_REPORT.md`

```markdown
# Final Report — Cryptocurrency Liquidity Prediction

## Executive Summary
We predict **liquidity ratio = 24h_volume / mkt_cap** to indicate near-term market tradability.
On cross-sectional data across two days (~1k coins/day), our best model (**RidgeCV on log-target**) achieves:
- **RMSE = 0.081**
- **MAE  = 0.034**
- **R²   = 0.954**
The deployed Flask app provides a minimal UI with professional design and interpretable output badges (Very Low → Very High).

## Problem Statement
Low liquidity leads to unstable prices and slippage. An early signal helps traders/exchanges manage risk.

## Data
- CoinGecko snapshots (2022-03-16, 2022-03-17)
- Cleaned and merged (993 rows)
- Engineered label: `liquidity_ratio`
- Heavy-tailed numeric features → log transforms applied

## Methodology
1. **EDA:** validated distributions, correlations, outliers
2. **Feature Engineering:** MAs, lag1 values, day-over-day changes, logs
3. **Modeling:** compare linear & tree ensembles with `log1p(y)` target
4. **Evaluation:** metrics + residuals; select best
5. **Serving:** Flask app with derived features, UX labels

## Features Used (X)
- Direct: `price, 1h, 24h, 7d, 24h_volume, mkt_cap`
- Trend/Vol: `price_ma_3, price_ma_5, vol_3d`
- Lags: `liquidity_ratio_lag1, price_lag1, 24h_volume_lag1, mkt_cap_lag1`
- Deltas: `price_ret_1d, vol_chg_1d, mcap_chg_1d`
- Logs: `log_price, log_vol, log_mcap`

## Results
- **Best model:** RidgeCV (scaled features, log-target)
- **Metrics:** RMSE 0.081, MAE 0.034, R² 0.954
- Residuals centered near 0; actual vs predicted ≈ diagonal

## Discussion
- Label is a pragmatic proxy; true liquidity = spread + depth + impact.
- With more dates, add temporal CV and order-book features for robustness.
- Thresholds (for UI badges) chosen from empirical distribution; can be recalibrated with more history.

## Deployment
- Flask app exposes a clean UI
- Minimal inputs; server infers engineered features; prediction shown with interpretation

## Limitations & Future Work
- Short time horizon (2 days) → extend to months
- Include exchange microstructure (spreads, depth, volatility of volume)
- Add social/news/exchange-listing signals
- Calibrate thresholds by quantiles per market-cap bucket

## Reproducibility
- Run notebooks in order: data prep → EDA → FE → modeling → evaluation
- Best model serialized at `artifacts/models/RidgeCV_logtarget.joblib`
- App start: `python app.py` → http://127.0.0.1:5000/

