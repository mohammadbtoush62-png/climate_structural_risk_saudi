Supplementary E – Monte Carlo scenario design and outputs
E.1 Purpose
The Monte Carlo experiments use the fitted GBT surrogate to compare how alternative enforcement, QA/QC, and digitalisation scenarios shift mean predicted Structural Risk (µ_s) and the share of high‑risk projects (π_s = P(ŷ ≥ τ)) for a large synthetic project population reflecting the joint distribution of drivers observed in the survey (N = 300).

E.2 Baseline joint distribution and sampling
Drivers: env, design, materials, enforcement.
Compute sample mean vector μ̂ and covariance matrix Σ̂ from the survey data (N = 300).
Generate N_sim = 10,000 draws z ~ N(μ̂, Σ̂) (fixed random seed), then truncate each component to [0,1] to form baseline driver vectors.
E.3 Scenario definitions
S0 (Baseline): x_S0 = x.
S_E (Enhanced enforcement): enforcement = min(1, enforcement + 0.20).
S_M (Materials/QA): materials = min(1, materials + 0.20).
S_D (Digitalisation): enforcement = min(1, enforcement + 0.10); materials = min(1, materials + 0.10); design = min(1, design + 0.05).
S_C (Combined): enforcement = min(1, enforcement + 0.20); materials = min(1, materials + 0.20); design = min(1, design + 0.10).
E.4 Risk prediction and metrics
For each synthetic draw and scenario, compute ŷ = f̂(x) using the trained GBT.
Compute µ_s = mean(ŷ) and π_s = mean(ŷ ≥ τ) with τ = 0.7.
Report Δµ_s = µ_s − µ_S0 and Δπ_s = π_s − π_S0.
E.5 Uncertainty via model‑bootstrap ensemble
B = 500 bootstrap resamples of the training set (n = 240). For each b:
Resample training data, refit GBT, and compute µ_s^(b), π_s^(b) using the same N_sim baseline draws.
Report median and 2.5–97.5 percentiles over b for each scenario metric. Save draws to /outputs/scenario_bootstrap_draws.csv.
E.6 Sensitivity analyses
Alternative Δ values (e.g., 0.10, 0.30) and τ ∈ {0.65,0.70,0.75}.
Alternative sampling: empirical resampling of observed rows with small noise.
Reported result: scenario ordering is robust (Combined > Enforcement > Materials > Digitalisation > Baseline).
