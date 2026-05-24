# Thesis replication code (updated) — Swedish electricity markets (Chapters 4.1–4.4)

This folder **`Thesis_code_new`** is the **current** replication package intended for a **new GitHub repository** (same layout as the earlier `Thesis_code` snapshot, with code synced from the active `chapter4_*` directories in the thesis workspace).

Each chapter is a self-contained Python package; paths assume **this directory is the repository root** (sibling folders `chapter4_spot_arbitrage`, `chapter4_balancing`, `chapter4_regression`, `chapter4_scenario_analysis`).

---

## Repository layout

| Folder | Content |
|--------|---------|
| `chapter4_spot_arbitrage/` | ENTSO-E day-ahead prices (SE3, SE4), HICP deflation, native-resolution daily PuLP arbitrage, figures & summary tables (Section 4.1). |
| `chapter4_balancing/` | ENTSO-E activated balancing energy (A84: aFRR/mFRR) and contracted reserve capacity (A81: aFRR/mFRR/FCR), preprocessing, native-resolution upper-bound optimisation, figures & tables (Section 4.2). |
| `chapter4_regression/` | Daily panel from spot + balancing outputs; VIF; Ridge / Lasso / Elastic Net; Random Forest / Gradient Boosting; LaTeX-ready tables (Section 4.3). |
| `chapter4_scenario_analysis/` | Stylised Day-Ahead scenarios, spot re-optimisation, proxy for balancing-side revenues, tornado-style sensitivity (Section 4.4). |

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

Run from **this repository root** (`Thesis_code_new/`), or use **`RUN_FULL_THESIS_PIPELINE.ps1`** (PowerShell, Windows).

1. **Spot (4.1)** — `cd chapter4_spot_arbitrage` then `python main.py` (or `python main.py --skip-fetch` if `data/processed_prices.parquet` exists).
2. **Balancing (4.2)** — `cd ../chapter4_balancing` then `python main_bal.py` (or `--skip-fetch` when caches exist).
3. **Regression (4.3)** — `cd ../chapter4_regression` then `python main_rebuilt.py`.
4. **Scenario (4.4)** — `cd ../chapter4_scenario_analysis` then `python main_scenario.py` (optional `--skip-tornado`).

**Figures only (cached data):** `.\REGENERATE_FIGURES_ONLY.ps1` from this root.

---

## Data sources (summary)

- **ENTSO-E** via `entsoe-py` (day-ahead A44; activated balancing energy A84; contracted reserves A81 as in each chapter’s fetch scripts).
- **Inflation:** Eurostat HICP `prc_hicp_midx` (Sweden CP00), aligned across chapters.

See **`APPENDIX_A_libraries_and_repository.md`** for libraries and solver notes.

---

## Notes for GitHub

- Prefer a **new repo** (e.g. `BESS_Thesis_new`) pointing at this folder’s contents as `main`, or rename on push as you prefer.
- **No API keys** in the repo: `ENTSOE_API_KEY` is read from the environment only.
- Large generated files (`*.parquet`, long CSVs) may stay ignored or use Git LFS — see `.gitignore` and uncomment ignores if needed.

---

## 中文简介

本目录为论文 **第 4 章** 当前版本的 **GitHub 用代码包**，结构与旧版 `Thesis_code` 相同，代码从工作区 `chapter4_*` **同步**而来。配置 **`ENTSOE_API_KEY`** 后按顺序运行各章 `main` 脚本即可复现；详见上文英文步骤与根目录 PowerShell 脚本。

---

*Update this README’s GitHub URL in `APPENDIX_A_libraries_and_repository.md` when the new repository is published.*
