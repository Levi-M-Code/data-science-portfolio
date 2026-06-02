# ReCell — Used Device Price Prediction with Linear Regression

## Business Problem

The global used and refurbished device market was projected to reach $52.7 billion by 2023, yet pricing in this market remains largely inconsistent and manual. ReCell, a startup operating in this space, needed a data-driven pricing model to support a dynamic pricing strategy for used phones and tablets.

This project analyzes 3,454 used device records across 33 brands to build a linear regression model that predicts normalized resale price and identifies the key factors that drive it.

---

## Dataset

| Feature | Description |
|---|---|
| `brand_name` | Device manufacturer |
| `os` | Operating system |
| `screen_size` | Screen size in cm |
| `4g` / `5g` | Connectivity capability |
| `main_camera_mp` | Rear camera resolution in MP |
| `selfie_camera_mp` | Front camera resolution in MP |
| `int_memory` | Internal storage in GB |
| `ram` | RAM in GB |
| `battery` | Battery capacity in mAh |
| `weight` | Device weight in grams |
| `release_year` | Year of original release |
| `days_used` | Days the device has been in use |
| `normalized_new_price` | Normalized new device price (EUR) |
| `normalized_used_price` | Normalized used device price (EUR) — **target variable** |

**Source:** Course-provided dataset, Post Graduate Program in Data Science with Generative AI: Applications to Business — University of Texas at Austin McCombs School of Business

---

## Tools Used

- Python (pandas, numpy, statsmodels, scikit-learn, scipy)
- Matplotlib, Seaborn
- Jupyter Notebook

---

## Methodology

| Step | Approach |
|---|---|
| Missing value treatment | Brand- and attribute-filtered subset imputation (median/mean based on distribution) |
| Feature engineering | Converted `release_year` to `years_past_release` for interpretability |
| Dummy encoding | One-hot encoding with `drop_first=True` for all categorical variables |
| Multicollinearity | VIF analysis; dropped `os_iOS` due to structural redundancy with `brand_name_Apple` |
| Feature selection | Automated backward elimination on p-values (α = 0.05) |
| Assumption testing | Linearity (residual plot), normality (Shapiro-Wilk, Q-Q), independence (Durbin-Watson), homoscedasticity (Goldfeld-Quandt) |

---

## Model Performance

| Metric | Train | Test |
|---|---|---|
| R² | 0.845 | 0.843 |
| Adjusted R² | 0.841 | 0.839 |
| RMSE | ~0.09 | ~0.09 |
| MAE | ~0.06 | ~0.06 |
| MAPE | ~4% | ~4.5% |

The model explains over 84% of variance in used device prices on both training and test data with no meaningful overfitting. Predictions are accurate to within approximately 4–5%.

---

## Key Findings

- **Normalized new price is the strongest predictor** of used price (coef: 0.42) — a device's original market positioning strongly anchors its resale value.
- **Screen size, main camera, selfie camera, and RAM** are all significant positive predictors of used price.
- **4G capability adds measurable resale value** (coef: 0.043) independent of other features.
- **Days used and battery capacity** are not significant predictors once other factors are controlled — contrary to common assumptions about device wear.
- **Data quality issues were identified** — including likely unit inconsistencies in screen size and internal memory fields, and at least one confirmed battery entry error (500 mAh vs. an expected 5,000 mAh for the same device model).

---

## Business Recommendations

1. **Prioritize procurement of high-original-price, large-screen, high-camera-resolution devices** — these features most reliably predict strong resale value.
2. **Deploy the current model for dynamic pricing** — with R² > 0.84 and ~4% MAPE, it is ready for production use as a pricing guidance tool.
3. **Invest in data quality** — verify screen size units and request a corrected dataset; even minor entry errors were identified that could affect model reliability at scale.
4. **Consider separate models for phones and tablets** — the dataset contains two distinct device populations with meaningfully different physical characteristics that a single model must accommodate.

---

## Notable Implementation Details

- Implemented reusable utility functions for distribution analysis, VIF computation, multicollinearity impact testing, and backward elimination — keeping the notebook clean and the methodology reproducible.
- Missing value imputation was performed using filtered subsets of like devices (matched on brand, OS, connectivity, and camera specs) rather than global statistics, improving imputation accuracy for device sub-populations with distinct characteristics.
- Identified and investigated anomalous Apple devices running non-iOS operating systems — a small but analytically interesting edge case suggesting a niche market for modified devices.

---

## Skills Demonstrated

- Linear regression modeling (OLS via statsmodels)
- Missing value imputation with domain-informed subset filtering
- Feature engineering and dummy variable encoding
- Multicollinearity diagnosis and treatment (VIF)
- Full linear regression assumption testing
- Model performance evaluation (R², Adj R², RMSE, MAE, MAPE)
- Reusable function design for clean, reproducible analysis

---

*This project was completed as part of the Post Graduate Program in Data Science with Generative AI: Applications to Business through the University of Texas at Austin McCombs School of Business. Full code written independently from scratch using the full-code submission path rather than the provided low-code template.*
