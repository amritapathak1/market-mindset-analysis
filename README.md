# market-mindset-analysis

## Experiments

| Experiment Key | Additional Information Available | Additional Information Cost | Profit/Loss Shown After Task |
| --- | --- | --- | --- |
| e1 | Yes | Fixed | Yes |
| e2 | Yes | Dynamic | Yes |
| e3 | No | - | Yes |
| e4 | Yes | Fixed | No |
| e5 | Yes | Dynamic | No |
| e6 | No | - | No |

`e6` is the control condition. The others are treatment conditions.

Dynamic pricing is generally related to company size. The larger the company, the smaller the cost of additional information.

## Data

| File | Description |
| --- | --- |
| `participants.csv` | One row per participant: experiment key, completion status, withdrawal status, and timestamps. |
| `task_responses.csv` | One row per participant-task: stock shown, investment amounts, remaining budget, and condition flags (`show_profit_loss`, `show_information`). |
| `portfolio.csv` | One row per investment: stock name/ticker, amount invested, return percentage, and profit/loss. |
| `confidence_risk.csv` | Per-task confidence and risk ratings, plus the attention check response used for eligibility filtering. |
| `events_raw.csv` | Raw click-level event log: every page view, button press, and information purchase during the experiment. |
| `events_cleaned.csv` | Cleaned version of the event log with tutorial events retained but flagged for easy exclusion. |
| `demographics.csv` | Self-reported age, gender, education, income, employment, and prior investment experience. |
| `info_costs.csv` | Cost of additional information per stock ticker per experiment key (used in dynamic-pricing conditions). |
| `stock_risk.csv` | Binary risk classification (high/low) for each stock ticker. |
| `feedback.csv` | Free-text feedback submitted by participants at the end of the experiment. |

## Analysis

### `main_analysis/`

| Notebook | Description |
| --- | --- |
| `01_data_prep.ipynb` | Filters eligible participants (attention checks, completion), assigns condition variables, and builds cleaned participant- and round-level DataFrames saved to `data/processed/`. |
| `02_behavioral_modeling.ipynb` | Tests hypotheses H1–H5 with OLS regression: earnings and investment rate as outcomes, with `info_type × show_pl` interaction (H4). |
| `03_calibration.ipynb` | Computes Brier scores and quadratic scoring rule per round; tests H6 (whether higher self-reported confidence predicts worse calibration) and evaluates mechanism welfare. |
| `04_segmentation.ipynb` | K-means clustering and Gaussian mixture models on behavioral features (risk-taking, info-seeking, confidence, volatility) to identify investor profiles. |

### `prelim_analysis/`

| Notebook | Description |
| --- | --- |
| `anova.ipynb` | One-way ANOVAs with assumption checks (Shapiro-Wilk, Levene) on earnings, investment rate, info use, and confidence across the six conditions. Initial pre-analysis. |
| `extended_analysis.ipynb` | Chi-square and binomial regressions on information purchasing behavior; deeper earnings breakdowns by `info_type × show_pl`. |
| `workshop_analysis.ipynb` | Descriptive tables, grouped bar charts with 95% CIs, normality/variance checks, and two-way ANOVAs prepared for workshop presentation. |

### `scripts/`

| File | Description |
| --- | --- |
| `proper_scoring.py` | Implements `brier_score()` and `quadratic_score()` used in `03_calibration.ipynb` to measure forecast accuracy. |
