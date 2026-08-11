# E337 — Machine Learning for Economists (UdeSA, 2026)

Coursework repository for E337, Universidad de San Andrés. Group 1
(Constanza Efkhanian, Julián Notario, María Florencia Pascale).

Two bodies of work:

1. **TP1–TP5**, a five-part series predicting labour informality from
   Argentine household survey microdata.
2. **Proyecto_poster**, an independent project predicting migration
   aspirations in Nigeria.

Everything is written in Python (pandas, NumPy, scikit-learn, matplotlib) in
Jupyter notebooks. Reports and the poster are in Spanish; code is readable in
either language.

---

## TP1–TP5 — Predicting labour informality (EPH, Greater Buenos Aires)

Microdata from Argentina's Permanent Household Survey (Encuesta Permanente de
Hogares, INDEC), pooling the third quarter of 2005 and the third quarter of
2025 for the Greater Buenos Aires region. The task is to predict whether an
employed person works informally, and to compare classification methods on it.

**TP1 — Data construction and cleaning.**
Pooling the two waves into a single dataset. They share variable names but not
types or codings twenty years apart, and non-response is encoded as ordinary
integers, so most of the work is harmonisation and validation rather than
analysis. Builds the `informal` indicator from employment category and pension
contributions, and compares it against the direct informality variable the
survey has reported since 2023. Missing-value heatmaps and first descriptives.

**TP2 — Feature construction and unsupervised methods.**
Years of schooling derived from the survey's education questions, hours worked,
and total household income deflated from 2005 to 2025 pesos. Histograms and
kernel density estimates by formal/informal status. PCA on the six continuous
predictors with score, loading and explained-variance plots. K-means at
k = 2, 4 and 10 with an elbow diagnostic, plus hierarchical clustering.

**TP3 — Classification.**
Train/test split by year. Logistic regression with coefficients, standard
errors and odds ratios, and predicted-probability plots. KNN at K = 10, 50 and
100 with decision boundaries, and K selected by 5-fold cross-validation.
Confusion matrices, ROC curves and out-of-sample metrics.

**TP4 — Regularisation.**
LASSO and Ridge logistic regression over a penalty grid, with the optimal
penalty chosen by 5-fold cross-validation and box plots of the validation error
at each value. Coefficient comparison against the unpenalised logit. 2025 only.

**TP5 — Trees and model comparison.**
CART pruned by cost-complexity, with `ccp_alpha` selected by 10-fold
cross-validation. Tree visualisation and predictor importances, checked against
which coefficients LASSO shrank to zero. Final comparison of logit, KNN, LASSO,
Ridge and CART, framed around targeting a labour formalisation programme, where
the relevant question is which type of error to tolerate rather than which model
maximises accuracy.

---

## Proyecto_poster — Migration aspirations in Nigeria

*¿Me quedo o me voy? Aspiraciones migratorias en Nigeria*

Can an adult's stated intention to migrate be predicted from observable
characteristics, and do non-linear models beat linear ones on this task?

**Data.** Nigeria General Household Survey 2023/2024, both the post-planting and
post-harvest visits. Individuals aged 15 and over, n = 5,020.

**Models.** Logit as a baseline, Elastic Net for variable selection, and Random
Forest as the final model. Train and test are split **by household** rather
than by individual, so that people from the same household cannot appear on
both sides of the split.

**Results.** Random Forest reaches AUC 0.700 against 0.656 for logit and 0.658
for Elastic Net, with recall of 0.762 against roughly 0.62 for both linear
models. The gap is statistically significant, which is the substantive finding:
the relationship between the covariates and migration aspiration is not linear.
Since the policy-relevant error here is failing to identify someone who does
intend to migrate, recall matters more than accuracy.

**What carries the signal.** Permutation importance (mean drop in AUC) puts age
far ahead of everything else, with aspiration falling from around 35 onward.
Internal remittances, region, school attendance and marital status follow at a
much lower level. Individual shocks each contribute modestly; among them,
climate shocks are the most associated with wanting to migrate.

**Limitations.** Prediction is not causality, the linear signal is weak and
important variables are unobserved, external validity beyond this sample is
unclear, and stated intention is not the same thing as actual migration.

---

## Data

EPH microdata is public and downloadable from INDEC
(https://www.indec.gob.ar). The Nigeria GHS is public and distributed by the
World Bank LSMS. Raw data files are not committed to this repository.
