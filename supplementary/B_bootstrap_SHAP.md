Supplementary B – Bootstrap and SHAP stability
B.1 Bootstrap protocol for performance metrics
Purpose: quantify uncertainty in test‑set performance (n = 60) for OLS, Random Forest, and GBT.
Procedure:
Use the held‑out test set.
B = 500 non‑parametric bootstrap resamples (sample n=60 with replacement).
For each resample, keep the trained model fixed, predict, and compute R² and MAE.
Report median and 2.5–97.5 percentile intervals for R² and MAE.
Outputs: /outputs/metrics_bootstrap_draws.csv.
B.2 Bootstrap protocol for scenario outcomes
Purpose: propagate surrogate/model uncertainty into scenario summaries.
Procedure:
B = 500 bootstrap resamples of the training set (n = 240); for each b:
Resample training data with replacement.
Refit GBT using best hyperparameters from Supplementary A.
Use the same baseline synthetic population (N_sim) and scenario adjustments to compute µ_s^(b) and π_s^(b).
Report median and 95% percentile intervals across b for each scenario metric.
Outputs: /outputs/scenario_bootstrap_draws.csv.
B.3 SHAP computation
Explainer: shap.TreeExplainer on final GBT.
Background dataset: training set (n = 240).
Compute per‑observation SHAP values for all observations (n = 300) and derive global importance (mean |SHAP|).
B.4 SHAP stability (bootstrap)
Purpose: assess robustness of feature‑importance ordering.
Procedure:
B = 200 bootstrap resamples of the training set.
For each b: refit GBT, compute SHAP on full dataset, record mean |SHAP| per feature.
Summarize median and 95% intervals of mean |SHAP| and empirical rank frequencies.
Key result summary (to include in main text / Figure 2 caption):
Enforcement is top‑ranked in the majority of bootstrap runs; Materials/QA and Environment follow; Design–Execution and controls are lower and stable.
Outputs: /outputs/shap_bootstrap_summary.csv and /outputs/shap_bootstrap_plots/.
