# Cryptocurrency Liquidity — EDA Report

## Dataset
- Source: CoinGecko daily snapshots (2022-03-16, 2022-03-17)
- Rows: 993 | Columns: 11 (post-clean)
- Columns: `coin, symbol, price, 1h, 24h, 7d, 24h_volume, mkt_cap, date, source_file, liquidity_ratio`
- Target proxy: `liquidity_ratio = 24h_volume / mkt_cap`

## Data Quality
- Dropped 7 rows with critical NA.
- No remaining missing values in core fields.
- Liquidity ratio stats:
  - min = 0.0000, p50 = 0.0338, p75 = 0.0883, max = 5.9485 (right-skewed)

## Distributions (key)
- `% changes (1h, 24h, 7d)`: centered near 0; long tails on 7d.
- `price, 24h_volume, mkt_cap`: heavy-tailed; log transform recommended.

## Correlations (high-level)
- `24h_volume` ↔ `mkt_cap`: positive (bigger caps trade more).
- `liquidity_ratio` correlates more with `% changes` and `24h_volume` than price.

## Notable Plots
- Correlation heatmap
- Histograms: `price`, `24h`, `7d`, `24h_volume`, `mkt_cap`, `liquidity_ratio`
- Scatter vs target: `1h`, `24h`, `7d`, `24h_volume`, `mkt_cap`, `price`

> See `/reports/figures/` or notebook images generated during EDA.
