Climate‑Resilient Structural Performance in Northern Saudi Arabia
Repository contents:

/notebooks/ — Colab notebooks reproducing analyses (train/test splits, CV, model fitting, SHAP, Monte Carlo scenarios).
/data/survey_data.csv — Analysis dataset (survey of 300 construction professionals).
/models/gbt_final.joblib — Saved GBT surrogate model.
/outputs/ — Result artifacts: metrics_bootstrap_draws.csv, shap_bootstrap_summary.csv, scenario_bootstrap_draws.csv, scenario_summary.csv.
/supplementary/ — Supplementary materials:
A_model_tuning.md
B_bootstrap_SHAP.md
E_monte_carlo_scenarios.md
data_description.md (if present)
Reproducibility

Open the notebook in /notebooks and run all cells (Runtime → Run all). Ensure survey_data.csv is present in /data or repo root.
Fixed random seeds are used (see notebooks) to ensure deterministic results.
Large model‑bootstrap steps (B up to 500) may increase runtime; reduce B in the notebook for faster runs.
Data access

survey_data.csv included is anonymized survey responses used for analyses. For additional data requests, contact the corresponding author.
