# Thesis replication code — Swedish electricity markets (Chapters 4.1–4.4)

Replication package for empirical chapters: **day-ahead spot arbitrage**, **aFRR balancing**, **regularized regression & tree models**, and **price scenario / sensitivity analysis**.  
Each chapter is a self-contained Python package under its own folder; paths assume **this directory is the repository root** (sibling folders `chapter4_spot_arbitrage`, `chapter4_balancing`, `chapter4_regression`, `chapter4_scenario_analysis`).

---

## Repository layout

| Folder | Content |
|--------|---------|
| `chapter4_spot_arbitrage/` | ENTSO-E day-ahead prices (SE3, SE4), HICP deflation, daily linear-programming arbitrage, figures & summary tables (Section 4.1). |
| `chapter4_balancing/` | ENTSO-E A84 activated aFRR prices (process A16, business A96), cleaning, hourly LP balancing revenue (Section 4.2). |
| `chapter4_regression/` | Daily panel from spot + balancing outputs; VIF, Ridge/Lasso/ElasticNet, Random Forest / Gradient Boosting; LaTeX-ready tables (Section 4.3). |
| `chapter4_scenario_analysis/` | Synthetic price scenarios, spot re-optimisation, Lasso proxy for balancing revenue, tornado-style sensitivity (Section 4.4). |

---

## Environment

```bash
python -m venv .venv
# Windows: .venv\Scripts\activate
# Unix:   source .venv/bin/activate
pip install -r requirements.txt
```

Register an [ENTSO-E Transparency](https://transparency.entsoe.eu/) account and create an API token. **Do not commit the token.**

```bash
set ENTSOE_API_KEY=your_token_here    # Windows cmd
$env:ENTSOE_API_KEY="your_token"      # Windows PowerShell
export ENTSOE_API_KEY=your_token      # Linux / macOS
```

---

## Run order (full replication)

Run from the **repository root** (`Thesis_code/`), with each `main` using `cd` into the chapter or `python path/to/main.py`.

1. **Spot (4.1)** — fetches DA prices (unless cached), runs daily PuLP optimisation.

   ```bash
   cd chapter4_spot_arbitrage
   python main.py
   # Or without API (requires existing data/processed_prices.parquet):
   python main.py --skip-fetch
   ```

2. **Balancing (4.2)** — fetches aFRR activated prices, preprocesses, runs daily LP. Uses spot `daily_results_*.csv` for some figures.

   ```bash
   cd ../chapter4_balancing
   python main_bal.py
   # Cached hourly Parquet only:
   python main_bal.py --skip-fetch
   ```

3. **Regression (4.3)** — reads outputs from 4.1 and 4.2 (`processed_prices.parquet`, `daily_balancing_*.csv`, etc.).

   ```bash
   cd ../chapter4_regression
   python main_rebuilt.py --zone both
   ```

4. **Scenario analysis (4.4)** — builds scenarios from historical spot series, re-runs spot LP, fits balancing proxy.

   ```bash
   cd ../chapter4_scenario_analysis
   python main_scenario.py
   # Faster dry run (skip tornado figure):
   python main_scenario.py --skip-tornado
   ```

`--quick` flags on 4.1 / 4.2 restrict to a shorter sample window for testing.

---

## Data sources (summary)

- **Electricity prices:** ENTSO-E via `entsoe-py` (day-ahead; activated balancing energy document **A84**, aFRR **A96**, process type **A16** for Sweden).
- **Inflation (real EUR):** Eurostat HICP (`prc_hicp_midx`, Sweden CP00), same logic in spot and balancing pipelines.

Details for thesis wording and known caveats (e.g. Transparency web UI vs API) can be documented in your main thesis repository.

---


