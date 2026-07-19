# EasyVisa — Visa Approval Prediction with Ensemble Methods

## Business Problem

The U.S. Office of Foreign Labor Certification (OFLC) processed nearly 1.7 million visa applications in FY2016 — a 9% increase year-over-year. Manually reviewing every application is increasingly unsustainable. EasyVisa was engaged to build a machine learning solution to shortlist candidates with higher probabilities of approval, helping OFLC prioritize cases and formulate data-driven certification policies.

This project analyzes 25,480 visa applications, builds and compares five ensemble models across three data treatments, tunes the top performers, and delivers a production-ready model with actionable policy recommendations.

---

## Dataset

| Feature | Description |
|---|---|
| `continent` | Applicant's continent of origin |
| `education_of_employee` | Education level (High School, Bachelor's, Master's, Doctorate) |
| `has_job_experience` | Prior work experience (Y/N) |
| `requires_job_training` | Job training required (Y/N) |
| `no_of_employees` | Number of employees at sponsoring company |
| `yr_of_estab` | Year company was established → engineered to `company_age` |
| `region_of_employment` | Intended U.S. region of employment |
| `prevailing_wage` | Wage offered, in varying units |
| `unit_of_wage` | Wage unit (Hourly, Weekly, Monthly, Yearly) |
| `full_time_position` | Full-time or part-time position |
| `case_status` | **Target variable** — Certified or Denied |

**Source:** Course-provided dataset, Post Graduate Program in Data Science with Generative AI: Applications to Business — University of Texas at Austin McCombs School of Business

---

## Tools Used

- Python (pandas, numpy, scikit-learn, xgboost, imbalanced-learn, scipy)
- Matplotlib, Seaborn
- Jupyter Notebook

---

## Methodology

| Step | Approach |
|---|---|
| Feature engineering | Converted `yr_of_estab` to `company_age` using 2016 as reference year (dataset collection year), preventing metric drift on future reruns |
| Wage treatment | Attempted cross-unit annualization; abandoned after statistical investigation revealed hourly wages distributed uniformly rather than in a realistic contractor-premium pattern, invalidating the 40hr/week assumption |
| Outlier treatment | Corrected 33 sign-corrupted negative employee counts via absolute value; retained legitimate high-value outliers across wages and company size |
| Class imbalance | Compared original data, SMOTENC oversampling, and random undersampling across all models |
| Models built | Bagging, Random Forest, AdaBoost, Gradient Boosting, XGBoost |
| Tuning | RandomizedSearchCV with continuous parameter distributions, F0.5 scoring, 5-fold CV, n_iter=100 |
| Metric | **F0.5** — precision-weighted F-score chosen to reflect asymmetric cost structure: false certifications carry higher cost per INA's worker-protection mandate |

---

## Model Performance Summary

| Model | Data | Val F0.5 | Val Precision | Val Recall | Val ROC-AUC |
|---|---|---|---|---|---|
| GradientBoosting (untuned) | Original | 0.8009 | 0.7846 | 0.8734 | 0.7910 |
| GradientBoosting (tuned) | Undersampled | 0.8085 | 0.8319 | 0.7268 | 0.7875 |
| **AdaBoost (tuned)** | **Undersampled** | **0.8089** | 0.8351 | 0.7186 | 0.7899 |
| AdaBoost (tuned) — **Test** | Undersampled | **0.7949** | 0.8144 | 0.7256 | 0.7658 |

**Selected model: Tuned AdaBoost (Undersampled)** — best validation F0.5 score with strong generalization to the held-out test set.

*Note: GradientBoosting on original data achieved the highest F1 score (0.8266) and is offered as an alternative if the client requires equal weight between precision and recall.*

---

## Key Findings

- **Education is the strongest demographic predictor** — Doctorate holders are approved at 87.2% vs. 34.0% for High School applicants.
- **Job experience substantially improves approval odds** — 74.5% approval rate with experience vs. 56.1% without.
- **Prevailing wage, High School education, and company size** are the top three features by importance in the final model.
- **Wage unit scaling is unreliable in this dataset** — weekly, monthly, and yearly wage distributions overlap in ways that are statistically inconsistent with real compensation structures, making cross-unit normalization invalid.
- **Midwest has the highest regional approval rate (75.5%)** while Island has the lowest (60.3%), though Island represents only 0.8% of applications — interpret cautiously.
- **Hourly wage roles are certified at only 34.6%** vs. 69.9% for yearly wage roles — the largest approval gap among any categorical variable.

---

## Business Recommendations

1. **Deploy the tuned AdaBoost model for triage shortlisting** — F0.5 of 0.7949 on the held-out test set with precision-weighted scoring aligned to INA's worker-protection mandate.
2. **Offer GradientBoosting as an alternative configuration** — highest F1 score (0.8266) for clients who want to minimize total misclassification rather than weight precision above recall.
3. **Validate the underlying data before production deployment** — identified anomalies include sub-minimum-wage yearly salaries (6.9% of records), implausible weekly/monthly wage values, and wage unit distributions inconsistent with real compensation structures. A cleaner dataset would likely yield meaningfully better model performance.
4. **Use education and job experience as fast-filter criteria** — these two features alone create substantial approval rate stratification and could support simple rule-based pre-screening upstream of the ML model.

---

## Notable Implementation Details

- Investigated wage normalization rigorously before abandoning it — hourly wage distribution was statistically uniform rather than showing the expected contractor-premium right skew, invalidating the 40hr/week annualization assumption.
- Selected F0.5 as the evaluation metric based on a structured cost-benefit argument grounded in the INA's stated purpose of protecting US workers from adverse wage and labor impacts — false certifications carry a higher cost than false denials in this context.
- Used SMOTENC rather than plain SMOTE to handle categorical features correctly in oversampling — plain SMOTE would produce fractional values on one-hot encoded binary columns.
- Built reusable utility functions for model evaluation, results aggregation, and visualization, enabling clean comparison across 15 model-data combinations before selecting candidates for tuning.

---

## Skills Demonstrated

- Ensemble modeling (Bagging, Random Forest, AdaBoost, Gradient Boosting, XGBoost)
- Hyperparameter tuning via RandomizedSearchCV with continuous distributions
- Class imbalance handling (SMOTENC oversampling, random undersampling)
- Business-driven metric selection (F0.5 with documented cost-benefit rationale)
- Feature engineering with temporal reference anchoring
- Data quality investigation and documented treatment decisions
- Multi-model comparison framework with unified results tracking

---

*This project was completed as part of the Post Graduate Program in Data Science with Generative AI: Applications to Business through the University of Texas at Austin McCombs School of Business.*
