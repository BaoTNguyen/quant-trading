# Project Progress Tracker
Last updated: 2026-03-31

**Due: 2026-04-07, 6:00 PM Edmonton | 7 days remaining**
**Weight: 20% | Individual | 10-min presentation**

---

## Phase 1: Setup & Infrastructure
| # | Step | Status | Notes/Blockers |
|---|------|--------|----------------|
| 1.1 | Create public GitHub repo | Not Started | Must have daily commits — start TODAY |
| 1.2 | Initialize .qmd file with YAML header | Not Started | self-contained HTML output |
| 1.3 | Verify R packages installed | Not Started | tidyverse, tidyquant, tidymodels, tidyclust, timetk, TTR, PerformanceAnalytics, RTL, xts, quantmod, fredr, dlm/KFAS, foreach, doParallel, plotly, lattice, factoextra |
| 1.4 | Obtain/verify FRED API key | Not Started | Needed for fredr (yield curve, VIX, inflation) |
| 1.5 | Test API data pulls: all 11 sector ETFs + SPY | Not Started | Confirm data back to June 2000; handle XLC (2018) and XLRE (2015) |
| 1.6 | Test API data pulls: FRED macro indicators | Not Started | DGS10, DGS2, T10YIE, VIX, etc. |
| 1.7 | Push initial commit to GitHub | Not Started | Empty .qmd + context.md + progress.md |

## Phase 2: Data Acquisition & Preparation
| # | Step | Status | Notes/Blockers |
|---|------|--------|----------------|
| 2.1 | Load all sector ETF OHLCV via tq_get() | Not Started | From 2000-06-01 (or earliest available) |
| 2.2 | Adjust OHLC for splits/dividends | Not Started | quantmod::adjustOHLC() |
| 2.3 | Compute returns (retClCl, retOpCl, retClOp) per sector | Not Started | Course-mandated return types |
| 2.4 | Load macro indicators from FRED | Not Started | Yield curve spread, breakeven inflation, etc. |
| 2.5 | Merge price data + macro data on date index | Not Started | Handle missing dates (weekends, holidays) with na.locf |
| 2.6 | Define train/test split (Jun 2000–Dec 2024 / Jan 2025–Mar 2026) | Not Started | |
| 2.7 | Git commit: data pipeline complete | Not Started | |

## Phase 3: Exploratory Analysis
| # | Step | Status | Notes/Blockers |
|---|------|--------|----------------|
| 3.1 | Plot sector ETF price histories and returns | Not Started | Visual regime identification |
| 3.2 | Plot macro indicators over same period | Not Started | Yield curve, VIX, inflation |
| 3.3 | Correlation matrix: sector returns vs each other | Not Started | Identify co-movement patterns |
| 3.4 | Correlation: sector returns vs macro indicators | Not Started | Validate thesis — do indicators predict sector divergence? |
| 3.5 | Summary statistics by hand-labeled regimes (GFC, COVID, etc.) | Not Started | Sanity check before unsupervised clustering |
| 3.6 | Update strategy_context.md with EDA findings | Not Started | Section 1 (thesis validation) |
| 3.7 | Git commit: EDA complete | Not Started | |

## Phase 4: Feature Engineering & Kalman Filter
| # | Step | Status | Notes/Blockers |
|---|------|--------|----------------|
| 4.1 | Compute all indicator features (Layer 1 from context.md) | Not Started | Momentum, dispersion, correlation, relative strength, macro |
| 4.2 | Implement Kalman filter on indicator series | Not Started | Use dlm or KFAS package; local level model |
| 4.3 | Visualize raw vs Kalman-filtered indicators | Not Started | Confirm smoothing is appropriate — not too lagged |
| 4.4 | Build tidymodels recipe for feature preprocessing | Not Started | recipes::step_normalize(), handle NAs |
| 4.5 | Update strategy_context.md with final feature set | Not Started | |
| 4.6 | Git commit: features + Kalman filter | Not Started | |

## Phase 5: Clustering & Regime Detection
| # | Step | Status | Notes/Blockers |
|---|------|--------|----------------|
| 5.1 | Implement K-means clustering via tidyclust | Not Started | tidymodels workflow: recipe + workflow + fit |
| 5.2 | Determine optimal K (elbow method, silhouette, gap statistic) | Not Started | Test K = 2, 3, 4, 5 |
| 5.3 | Visualize clusters (PCA projection, cluster profiles) | Not Started | factoextra or ggplot |
| 5.4 | Interpret clusters: map to economic regimes | Not Started | What does each cluster mean? Risk-on, risk-off, etc. |
| 5.5 | Implement expanding-window clustering (avoid look-ahead bias) | Not Started | At each rebalancing date, only use data up to that date |
| 5.6 | Compute historical sector returns per cluster | Not Started | Which sectors outperform in each regime? |
| 5.7 | Update strategy_context.md with cluster interpretations | Not Started | |
| 5.8 | Git commit: clustering pipeline | Not Started | |

## Phase 6: Strategy Logic Implementation
| # | Step | Status | Notes/Blockers |
|---|------|--------|----------------|
| 6.1 | Create Mermaid mental model diagram | Not Started | MANDATORY — must match code logic |
| 6.2 | Implement signal generation: cluster → sector allocation → {+1, -1, 0} | Not Started | |
| 6.3 | Implement monthly rebalancing cadence | Not Started | Signal on last day of month → trade at open of first day of next month |
| 6.4 | Implement trade/position/P&L logic per sector | Not Started | ret_new, ret_exist, ret_others pattern |
| 6.5 | Aggregate per-sector returns to portfolio level | Not Started | Equal-weight across active positions |
| 6.6 | Compute cumulative equity (cumeq) | Not Started | cumprod(1 + ret) |
| 6.7 | Export and manually validate returns | Not Started | Spot-check in Excel |
| 6.8 | Wrap in strategy() function for optimization | Not Started | Parameters: n_clusters, momentum_lookback, n_long, n_short |
| 6.9 | Git commit: strategy logic | Not Started | |

## Phase 7: Optimization (Training Period)
| # | Step | Status | Notes/Blockers |
|---|------|--------|----------------|
| 7.1 | Define parameter grid via expand.grid() | Not Started | ~180 combinations |
| 7.2 | Set up foreach + doParallel multi-core loop | Not Started | |
| 7.3 | Run full optimization on training data | Not Started | Store RTL::tradeStats() per combination |
| 7.4 | Verify no errors/NA/Inf in results | Not Started | |
| 7.5 | Git commit: optimization results | Not Started | |

## Phase 8: Risk/Reward Assessment & Parameter Selection
| # | Step | Status | Notes/Blockers |
|---|------|--------|----------------|
| 8.1 | Visualize optimization surface (wireframe, plotly, heatmaps) | Not Started | CumReturn, Sharpe, DD.Max |
| 8.2 | Z-score normalize all metrics | Not Started | Course pattern |
| 8.3 | Define risk appetite thresholds | Not Started | Sharpe > 0.3, DD.Max > -40%, etc. |
| 8.4 | Filter results by risk appetite | Not Started | Identify robust clusters |
| 8.5 | Select final parameters with justification | Not Started | Document in strategy_context.md |
| 8.6 | Git commit: parameter selection | Not Started | |

## Phase 9: Backtesting (Out-of-Sample)
| # | Step | Status | Notes/Blockers |
|---|------|--------|----------------|
| 9.1 | Run strategy on test period (Jan 2025–Mar 2026) | Not Started | |
| 9.2 | Chart: price, trades, positions, cumulative equity | Not Started | |
| 9.3 | Compute tradeStats on backtest | Not Started | |
| 9.4 | Compare vs buy-and-hold SPY | Not Started | |
| 9.5 | Drawdown analysis | Not Started | |
| 9.6 | Correlation to underlying analysis | Not Started | |
| 9.7 | Git commit: backtest results | Not Started | |

## Phase 10: Document & Visualization Polish
| # | Step | Status | Notes/Blockers |
|---|------|--------|----------------|
| 10.1 | Write economic rationale narrative | Not Started | Section in .qmd |
| 10.2 | Write methodology section | Not Started | Clustering + Kalman + signal pipeline |
| 10.3 | Write results & discussion section | Not Started | Training + backtest |
| 10.4 | Write robustness discussion | Not Started | What could break the strategy? |
| 10.5 | Embed Mermaid diagram in .qmd | Not Started | |
| 10.6 | Ensure all required visualizations present | Not Started | Cross-ref project_requirements.md |
| 10.7 | Verify .qmd renders to self-contained HTML | Not Started | html_document: self_contained: true |
| 10.8 | Finalize context.md (graded deliverable) | Not Started | Must reflect final strategy |
| 10.9 | Git commit: final document | Not Started | |

## Phase 11: Presentation & Submission
| # | Step | Status | Notes/Blockers |
|---|------|--------|----------------|
| 11.1 | Prepare 10-minute presentation | Not Started | Thesis → method → results → conclusions |
| 11.2 | Practice timing | Not Started | 10 min strict |
| 11.3 | Final render of .qmd to HTML | Not Started | Verify all charts visible |
| 11.4 | Push final commit to GitHub | Not Started | |
| 11.5 | Email submission with GitHub repo link | Not Started | Due 2026-04-07 6pm EDM |
| 11.6 | Present in class | Not Started | 2026-04-07 |

