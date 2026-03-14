Supplementary A – Model tuning and cross‑validation
A.1 Data and features
All models are trained on the survey dataset with 300 observations and the following variables:

Continuous indices in [0,1]:  

struct_risk (target): Structural Risk Index
env: Environment index
design: Design–Execution integration index
materials: Materials/QA/QC index
enforcement: Enforcement/inspection index
Categorical controls:  

role ∈ {Civil engineer, Government official, Contractor, Consultant}
region ∈ {Northern, Central, Western, Eastern, Southern}
experience ∈ {6–10 years, 11–15 years, 16+ years}
Categorical variables are one‑hot encoded (dropping one column per group as reference). No standardization is applied to the [0,1] indices.

A.2 Train–test split
We split the data into training and test sets:

80% train (n = 240), 20% test (n = 60).
Random seed fixed at 42.
Stratification by region to preserve the north/non‑north composition.
All tuning and cross‑validation are performed only on the training set. The test set is held out for final evaluation.

A.3 Baseline linear model (OLS)
We estimate an OLS regression with:

Outcome: struct_risk
Predictors: env, design, materials, enforcement, plus one‑hot controls for role, region, experience.
We use statsmodels with robust (HC3) standard errors. The OLS model is not tuned; it serves as a transparent baseline. Performance on the test set is summarized in Table 4 (R² and MAE).

A.4 Tree‑based models and hyperparameter grids
We train two tree‑based models using scikit-learn:

Random Forest Regressor (RandomForestRegressor)
Gradient‑Boosted Trees (GradientBoostingRegressor, GBT)
For each, we perform grid‑search cross‑validation on the training set.

Random Forest grid

python

Collapse


 Copy

param_grid_rf = {
    'n_estimators': [200, 400, 600],
    'max_depth': [None, 6, 10],
    'min_samples_split': [2, 5],
    'min_samples_leaf': [1, 2],
    'max_features': ['sqrt', 0.8]
}
Gradient‑Boosted Trees grid

python

Collapse


 Copy

param_grid_gbt = {
    'n_estimators': [200, 400, 600],
    'learning_rate': [0.05, 0.1],
    'max_depth': [3, 4],
    'min_samples_split': [2, 5],
    'min_samples_leaf': [1, 2],
    'subsample': [0.8, 1.0],
    'max_features': ['sqrt', 0.8]
}
A.5 Cross‑validation procedure
Cross‑validation: 5‑fold CV on the training set.
Scoring metric: negative mean absolute error (neg_mean_absolute_error).
Implementation: GridSearchCV with n_jobs = -1, cv = 5, scoring = 'neg_mean_absolute_error', random_state = 42 where applicable.
For each grid:

Fit GridSearchCV on the training data.
Select the hyperparameters with the best mean CV score.
Refit the model on the full training set with these hyperparameters.
We record, for both RF and GBT:

Best hyperparameter combination.
Mean CV MAE and its standard deviation across folds.
A.6 Final model evaluation
The final models (OLS, RF, GBT) are evaluated on the held‑out test set (n = 60). For each model we compute:

R² on test data.
MAE on test data.
To quantify uncertainty, we bootstrap the test set:

B = 500 bootstrap resamples (sampling test observations with replacement).
For each resample, recompute R² and MAE.
Report the median and 95% percentile intervals for R² and MAE.
These summary metrics are shown in Table 4 of the main text:

OLS: lower R² and higher MAE.
RF: improved performance.
GBT: highest test R² and lowest MAE.
All tuning and evaluation code is provided in the Colab notebook in /notebooks, and is reproducible given the fixed random seeds and environment specified in the repository’s README.
