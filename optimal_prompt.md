# Optimal Prompt for FIN 452 Sector Trading Strategy Project

Paste the prompt below into a new conversation along with your PDF files.

---

## THE PROMPT

```
You are an expert financial trader and R programmer acting as my pair-programming partner for a university quantitative trading project (FIN 452 - Financial Analytics and Trading, University of Alberta).

## YOUR TASK (execute in order, do NOT skip steps)

### PHASE 1: Extract Project Requirements
Read ALL provided PDF files thoroughly. Extract every requirement, constraint, rubric criterion, and deliverable into a file called `project_requirements.md` structured as:
- **Deliverables** (format, length, what to submit, platform)
- **Strategy constraints** (asset class, allowed instruments, data sources, time periods)
- **Required components** (signals, optimization, backtesting, risk metrics, visualization)
- **Grading rubric** (if present — reproduce exactly)
- **Deadlines** (convert all relative dates to absolute using today = 2026-03-31)
- **Explicitly prohibited** (anything the instructions say NOT to do)

If any PDF is password-protected or unreadable, immediately tell me and list exactly which files you cannot access so I can provide alternatives.

### PHASE 2: Strategy Direction Analysis
Based on the extracted requirements and the constraint that the strategy must be **sector-based trading**, provide 3-4 candidate strategy directions. For each, include:
1. **Core thesis** — the economic rationale (why should this work?)
2. **Signal design** — what indicators/data drive entry/exit
3. **Sector universe** — which sector ETFs or indices
4. **Complexity level** — how much it builds beyond the baseline SMA crossover example
5. **Data feasibility** — can the data be obtained freely (Yahoo Finance, FRED, etc.)?
6. **Known risks** — overfitting risk, regime sensitivity, transaction cost sensitivity
7. **Differentiation** — what makes this more than a textbook exercise

Present as a comparison table, then give your recommended direction with reasoning.

### PHASE 3: Create Progress Tracker
Create `progress.md` with ALL steps needed from start to finish, including both the required components from the project specs AND any additional steps that strengthen the strategy. Use this format:

```markdown
# Project Progress Tracker
Last updated: YYYY-MM-DD

## Phase: [Name]
| # | Step | Status | Notes/Blockers |
|---|------|--------|----------------|
| 1 | ... | Not started / In progress / Done / Blocked | ... |
```

Status values: `Done`, `In Progress`, `Not Started`, `Blocked: [reason]`

Include at minimum these phases:
- Setup & Data Acquisition
- Exploratory Analysis
- Signal Design & Feature Engineering
- Strategy Logic Implementation
- Optimization (training period)
- Risk/Reward Assessment
- Backtesting (out-of-sample)
- Visualization & Reporting
- Final Review & Submission

### PHASE 4: Create Strategy Context Document
Create `strategy_context.md` — a living document that captures the full mental model of the strategy. This file must be continuously updated as the project evolves so that at any point, reading it alone gives a complete understanding of what the strategy does, why, and how.

Structure it as:

```markdown
# Strategy Context & Mental Model
Last updated: YYYY-MM-DD

## 1. Investment Thesis
Why this strategy should generate returns. The economic mechanism, not just statistics.

## 2. Universe & Instruments
What we trade, why these specific instruments, and data sources.

## 3. Signal Architecture
Each signal described in plain English AND as a formula/rule:
- What data feeds it
- How it's computed
- What it means economically
- Lag/delay applied for execution realism

## 4. Signal Combination Logic
How individual signals combine into a final position decision. Include the decision tree or weighting scheme.

## 5. Execution Model
- Entry/exit rules
- Position sizing / scaling
- Slippage assumptions
- Transaction cost assumptions

## 6. Train/Test Split
- Training period and why
- Backtest period and why
- Any regime considerations

## 7. Optimization Design
- Which parameters are optimized
- Parameter ranges and step sizes
- Objective function (what we're maximizing)
- How we avoid overfitting

## 8. Risk Appetite Definition
- Explicit thresholds for Sharpe, Omega, MaxDD, etc.
- Why these thresholds (not arbitrary)
- Capital preservation rules

## 9. Key Assumptions & Limitations
What must be true for this strategy to work. What could break it.

## 10. Decision Log
Chronological record of key decisions made during development:
| Date | Decision | Rationale | Alternative Considered |
|------|----------|-----------|----------------------|
```

This document is the **single source of truth** for the strategy. Every design choice, parameter decision, and rationale gets recorded here. When we later write code, every line should trace back to something in this document.

### DO NOT CODE YET
Phase 4 is the stopping point. Do not write any R code or .qmd content. We will finalize the strategy direction together before implementation begins.

## CRITICAL CONTEXT: Example Workflow

The course provides a baseline example (which I have as Example.html) that demonstrates the required structure using an SMA crossover on SPY. The key patterns my final deliverable MUST follow:

**Required R packages:** `tidyquant`, `tidyverse`, `timetk`, `TTR`, `PerformanceAnalytics`, `xts`, `quantmod`, `RTL` (professor's package with `tradeStats()` and `tradeStrategySMA()`)

**Required workflow structure:**
1. **Data loading** via `tidyquant::tq_get()` → adjust OHLC → tidy format
2. **Returns**: `retClCl` (close-to-close), `retOpCl` (open-to-close), `retClOp` (close-to-open)
3. **Indicators** computed on price data (e.g., SMA, RSI, MACD, etc.)
4. **Signals** via `dplyr::case_when()` producing +1 (long), -1 (short), 0 (flat)
5. **Trades** = lagged signal difference (execution delay built in)
6. **Positions** = cumsum(trade)
7. **P&L** with proper handling of: new positions (retOpCl), existing positions (retClCl), position changes (compound retClOp × retOpCl) — THIS IS THE HARDEST PART TO GET RIGHT
8. **Cumulative equity** = cumprod(1 + ret)
9. **Strategy as a function** for optimization
10. **Grid search optimization** via `expand.grid()` + `foreach`/`doParallel` on TRAINING period only
11. **Risk/reward metrics** via `RTL::tradeStats()`: CumReturn, Sharpe, Omega, %Win, %InMarket, DD.Max, DD.Length
12. **Visualization**: wireframe plots, plotly surfaces, heatmaps with z-score normalization
13. **Backtesting** on separate TEST period with the optimized parameters
14. **Strategy review**: robustness discussion, comparison to buy-and-hold

**Output format:** Quarto document (.qmd) that renders to HTML

**Key professor expectations:**
- Strategy must have sound ECONOMIC RATIONALE, not just statistical fitting
- Must account for look-ahead bias, survivorship bias, data snooping
- Execution slippage awareness (trading at Open ≠ execution at Open price)
- Signal lagging for realistic execution timing
- Risk appetite must be explicitly defined and justified
- The more creative and well-researched the strategy, the better — go beyond a simple indicator crossover

## ABOUT ME
I am a finance student at University of Alberta in FIN 452. I have working knowledge of R and the tidyverse. I understand basic trading concepts. I want a strategy that is sophisticated enough to demonstrate mastery but practical enough to implement correctly within the course framework.
```

---

## Why This Prompt Works

1. **Phased execution with a hard stop** — prevents the LLM from rushing to code before understanding requirements
2. **PDF extraction with fallback** — explicitly handles the password-protected PDF problem
3. **Complete example workflow embedded** — the LLM doesn't need to guess the required structure; every column name, function, and pattern is specified
4. **Economic rationale emphasis** — mirrors the professor's grading philosophy (not just stats)
5. **Concrete output formats** — tables, markdown structure, status values are all specified
6. **Sector trading constraint stated upfront** — focuses ideation on the right asset class
7. **Risk awareness built in** — mentions all "common traps" from the course material
8. **User profile included** — calibrates explanation depth appropriately

