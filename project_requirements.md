# FIN 452 — Quantitative Trading Project Requirements
Last updated: 2026-03-31

> **Source:** Project brief from professor (PDF content provided) + Chapter 8 of *Financial Analytics and Trading*.

---

## 1. Deliverables

| Item | Format | Details |
|------|--------|---------|
| Quarto source | `.qmd` file | All analysis in R, no Python |
| Rendered output | **Self-contained HTML** | All resources embedded. **-5 points** if professor has to chase you or re-render to see charts. |
| Strategy context | `context.md` | Living document capturing mental model — **graded deliverable** |
| GitHub repo | **Public** repo | Must include all code. **Daily commits** showing work journey. **-5 points** for single commit on submission day or lack of evidence of frequent work. |
| Presentation | 10 minutes, in class | Recorded for grading only (deleted after). May be peer-graded before professor grading. |

## 2. Deadlines

| Item | Date |
|------|------|
| **Due date** | **2026-04-07, 6:00 PM Edmonton time** |
| Presentation | In class on 2026-04-07 |
| Submission method | Email with GitHub repo link |
| Today | 2026-03-31 (**7 days remaining**) |

## 3. Grading

| Detail | Value |
|--------|-------|
| Weight | **20%** of course grade |
| Type | Individual assignment |
| Grading | Possibly peer-graded first, then professor |
| Penalties | -5 for non-self-contained HTML; -5 for no daily commit evidence; penalty for Python usage |

## 4. Hard Constraints (MANDATORY)

### 4.1 Asset Universe (FIXED — no choice)
| Ticker | Sector | Available From |
|--------|--------|---------------|
| **SPY** | S&P 500 (benchmark) | 1993 |
| XLB | Materials | 1998 |
| XLC | Communication Services | **2018** |
| XLE | Energy | 1998 |
| XLF | Financials | 1998 |
| XLI | Industrials | 1998 |
| XLK | Technology | 1998 |
| XLP | Consumer Staples | 1998 |
| XLRE | Real Estate | **2015** |
| XLU | Utilities | 1998 |
| XLV | Health Care | 1998 |
| XLY | Consumer Discretionary | 1998 |

### 4.2 Time Periods (FIXED)
| Period | Range |
|--------|-------|
| **Training** | **June 2000 – December 2024** |
| **Testing** | **January 2025 – March 2026** |

### 4.3 Strategy Method (MANDATORY)
| Requirement | Detail |
|-------------|--------|
| **Clustering** | **Must be used as PRIMARY method** to drive signals from indicator mental model |
| **Kalman Filter** | **Must be used IN CONJUNCTION with clustering** in triggering trade signals |
| Indicators | Free to use any indicators to augment strategy quality |
| Trading frequency | **Daily** |
| Rebalancing | **Monthly** |

### 4.4 Technical Requirements
| Requirement | Detail |
|-------------|--------|
| Language | **R only** — Python penalized |
| Document | **Quarto** (.qmd) |
| Workflow | **Must follow `tidymodels` workflow** |
| Data | **All data via APIs** — no local CSV/Excel files |
| Mental model | **Must be visualized using Mermaid charts** (https://mermaid.ai) |
| Code-model match | Code must match the mental model diagram |
| Presentation | Professional, logical structure, effective visualization |

## 5. Required Components (from Chapter 8 + Project Brief)

### 5.1 Data & Returns
- [ ] Load all SPDR sector ETFs + SPY via `tidyquant::tq_get()`
- [ ] Adjust OHLC for splits/dividends via `quantmod::adjustOHLC()`
- [ ] Compute returns: `retClCl`, `retOpCl`, `retClOp`
- [ ] Handle XLC (from 2018) and XLRE (from 2015) availability gaps
- [ ] Load any additional indicator data via APIs (FRED, etc.)

### 5.2 Clustering (PRIMARY Signal Method)
- [ ] Select features/indicators for clustering input
- [ ] Implement clustering using `tidymodels` framework
- [ ] Clustering must drive the core trading signal logic
- [ ] Document cluster interpretation (what does each cluster mean economically?)

### 5.3 Kalman Filter (REQUIRED — with Clustering)
- [ ] Implement Kalman filter on relevant signal/indicator
- [ ] Use Kalman filter output in conjunction with clustering for trade signal generation
- [ ] Document how Kalman filter complements the clustering signal

### 5.4 Mermaid Mental Model Diagram
- [ ] Create Mermaid flowchart showing decision rules
- [ ] Diagram must show: data inputs → indicators → clustering → Kalman filter → signal → trade logic
- [ ] Code implementation must match the diagram exactly

### 5.5 Trade & Position Logic
- [ ] Signals: {+1 (long), -1 (short), 0 (flat)} per sector
- [ ] Lag signals for execution realism (signal on day T → trade at open T+1)
- [ ] Monthly rebalancing cadence
- [ ] `trade`, `pos`, `ret_new`, `ret_exist`, `ret_others`, `cumeq` columns (course pattern)

### 5.6 Strategy Function & Optimization
- [ ] Wrap strategy in function for optimization
- [ ] Parameter grid via `expand.grid()`
- [ ] Multi-core via `foreach` + `doParallel`
- [ ] Optimize on TRAINING period only (June 2000 – Dec 2024)

### 5.7 Risk/Reward Assessment
- [ ] `RTL::tradeStats()` metrics: CumReturn, Sharpe, Omega, %Win, %InMrkt, DD.Max, DD.Length
- [ ] Define and justify explicit risk appetite thresholds
- [ ] Filter and select from robust parameter clusters

### 5.8 Visualization
- [ ] Price charts with trades, positions, cumulative equity
- [ ] 3D wireframe / plotly surfaces for optimization
- [ ] Heatmaps with Z-score normalization
- [ ] Drawdown charts
- [ ] Correlation analysis
- [ ] **Mermaid decision flowchart** (mandatory)
- [ ] Cluster visualization (PCA/t-SNE or similar)

### 5.9 Backtesting
- [ ] Walk-forward on test period (Jan 2025 – Mar 2026) with optimized parameters
- [ ] Compare vs buy-and-hold (SPY)
- [ ] Full tradeStats on backtest
- [ ] Robustness discussion

### 5.10 Presentation
- [ ] 10-minute presentation prepared
- [ ] Clear narrative: thesis → method → results → conclusions
- [ ] Professional slides or rendered HTML walkthrough

## 6. Bias Warnings (from Chapter 8)

| Trap | How to Avoid |
|------|-------------|
| Survivorship bias | Use the fixed SPDR universe — ETFs rarely delist, but note XLC/XLRE start dates |
| Look-ahead bias | Lag all signals; economic data released in arrears |
| Data mining/snooping | Optimize on training only; backtest is untouched |
| Storytelling | Strategy must have economic rationale before fitting |
| Transaction costs | Acknowledge bid/offer, slippage |
| Outlier dependence | Check that performance isn't driven by 1-2 events |
| Adjusted prices | Use `adjustOHLC()` for splits/dividends |

## 7. Required R Packages

| Package | Purpose |
|---------|---------|
| `tidyverse` | Data wrangling |
| `tidyquant` | Yahoo Finance data |
| `tidymodels` | **Mandatory workflow** — recipes, parsnip, etc. |
| `timetk` | xts ↔ tibble conversion |
| `TTR` | Technical indicators |
| `PerformanceAnalytics` | Risk/return analytics |
| `xts` / `zoo` | Time series objects |
| `quantmod` | OHLC adjustment |
| `RTL` | Professor's package — `tradeStats()` |
| `lattice` | 3D wireframe |
| `plotly` | Interactive surfaces |
| `foreach` / `doParallel` | Multi-core optimization |
| `dlm` or `KFAS` or `FKF` | Kalman filter implementation |
| `cluster` / `factoextra` or tidymodels clustering | Clustering algorithms |

## 8. Reference Code Patterns (from Course Example)

These are the implementation patterns demonstrated in the course material. Our code must follow these conventions.

### 8.1 Data Loading & OHLC Adjustment
```r
dat <- tidyquant::tq_get("SPY", from = "2000-01-01") %>%
  dplyr::rename_all(tools::toTitleCase) %>%
  timetk::tk_xts(date_var = Date) %>%
  quantmod::adjustOHLC(., use.Adjusted = TRUE) %>%
  timetk::tk_tbl(rename_index = "Date") %>%
  dplyr::select(-Adjusted) %>%
  dplyr::mutate(across(where(is.numeric), round, 2)) %>%
  dplyr::rename(date = Date)
```
Key: convert to xts for adjustOHLC, then back to tibble for tidy work.

### 8.2 Three Return Types
```r
retClCl = Close / dplyr::lag(Close) - 1,        # close-to-close (held position)
retOpCl = (Close - Open) / Close,                # open-to-close (new position day)
retClOp = Open / dplyr::lag(Close) - 1,          # close-to-open (overnight gap)
```

### 8.3 Signal → Trade → Position → P&L Chain
```r
# Signals: +1 (long), -1 (short), 0 (flat)
signal = dplyr::case_when(condition1 ~ 1, condition2 ~ -1, TRUE ~ 0),

# Trade: lagged signal difference (1-day execution delay)
trade = tidyr::replace_na(dplyr::lag(signal) - dplyr::lag(signal, n = 2L), 0),

# Position: cumulative trades
pos = cumsum(trade),

# P&L: three components for correct return attribution
ret_new    = ifelse(pos == trade, pos * retOpCl, 0),                          # new position
ret_exist  = ifelse(pos != 0 & trade == 0, pos * retClCl, 0),                # held position
ret_others = dplyr::case_when(
  (pos - trade) != 0 & trade != 0 ~
    (1 + retClOp * (pos - trade)) * (1 + retOpCl * pos) - 1,                 # position change
  TRUE ~ 0
),
ret = ret_new + ret_exist + ret_others,

# Cumulative equity
cumeq = cumprod(1 + ret)
```

### 8.4 Strategy Function Template
```r
strategy <- function(data, par1, par2, ...) {
  # Do INSIDE: anything specific to this parameter combination
  # Do OUTSIDE: data loading, generic preprocessing
  data <- data %>%
    dplyr::mutate(
      # returns, indicators, signals, trades, positions, P&L
      ...
    )
  return(data)
}
```

### 8.5 Optimization Grid + Multi-Core Loop
```r
library(foreach); library(doParallel)

out <- expand.grid(par1 = seq(...), par2 = seq(...))

cl <- makeCluster(detectCores() - 1)
registerDoParallel(cl)

res <- foreach(
  i = 1:nrow(out), .combine = "cbind",
  .packages = c("tidyverse", "RTL", "timetk", "tidyquant", "PerformanceAnalytics")
) %dopar% {
  as.numeric(RTL::tradeStats(
    strategy(data = train, out[i, "par1"], out[i, "par2"]) %>%
      dplyr::select(date, ret)
  ))
}
stopCluster(cl)

res <- tibble::as_tibble(t(res))
colnames(res) <- names(RTL::tradeStats(x = check %>% dplyr::select(date, ret)))
out <- cbind(out, res)
```

### 8.6 tradeStats Output Columns
`RTL::tradeStats()` returns: `CumReturn`, `Ret.Ann`, `SD.Ann`, `Sharpe`, `Omega`, `%.Win`, `%.InMrkt`, `DD.Length`, `DD.Max`

### 8.7 Charting Pattern (xts-based)
```r
tmp <- result %>% timetk::tk_xts(date_var = date)
plot(tmp$Close, main = "Strategy Results")
xts::addSeries(tmp$trade, main = "Trades", on = NA, type = "h", col = "blue")
xts::addSeries(tmp$pos, main = "Positions", on = NA, type = "h", col = "blue")
xts::addSeries(tmp$cumeq, main = "CumEQ", on = NA, type = "l", col = "blue")
```

### 8.8 Optimization Visualization
```r
# 3D wireframe
lattice::wireframe(out$Sharpe ~ out$par1 * out$par2, shade = TRUE, drape = TRUE)

# Plotly surface
plotly::plot_ly(x = ~par2_vals, y = ~par1_vals, z = ~matrix) %>% add_surface()

# Heatmap with Z-score normalization
outZ <- out %>%
  tidyr::pivot_longer(cols = -c(par1, par2), names_to = "variable", values_to = "value") %>%
  dplyr::group_by(variable) %>%
  dplyr::mutate(valueZ = (value - mean(value)) / sd(value))

outZ %>% ggplot(aes(x = par1, y = par2)) +
  geom_raster(aes(fill = valueZ), interpolate = TRUE) +
  facet_wrap(~variable, scales = "free") +
  scale_fill_gradient2(low = "red", mid = "white", high = "blue", midpoint = 0)
```

### 8.9 Risk Appetite Filtering
```r
out %>%
  dplyr::filter(Sharpe > 0.2, DD.Max > -0.30) %>%
  dplyr::arrange(desc(Sharpe)) %>%
  top_n(10)
```

### 8.10 Drawdown Analysis
```r
sp.ret <- TTR::ROC(quantmod::Cl(xts_obj), type = "discrete")
PerformanceAnalytics::table.Drawdowns(sp.ret, top = 25)
PerformanceAnalytics::chart.Drawdown(sp.ret, main = "Drawdowns", col = "blue")
```

### 8.11 Correlation Analysis
```r
PerformanceAnalytics::chart.Correlation(
  data.frame(asset = retClCl, strategy = ret) %>% tidyr::drop_na(),
  histogram = TRUE
)
```

### 8.12 Key Performance Questions to Address
- What would buy-and-hold return? `last(Close) / first(Close)`
- What would risk-free return? (fixed horizon vs rolling daily rate)
- Strategy cumulative return: `RTL::tradeStats(...)["CumReturn"]`
- How robust is the strategy if market regime changes?
