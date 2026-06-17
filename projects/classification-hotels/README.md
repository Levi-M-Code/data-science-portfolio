# INN Hotels — Booking Cancellation Prediction

## Business Problem

INN Hotels Group, a hotel chain operating in Portugal, faces significant revenue loss from booking cancellations — particularly last-minute cancellations that leave rooms unsellable at full price. With 33% of bookings in this dataset canceled, the business needed a machine learning solution to predict cancellations in advance and inform smarter policies around overbooking, refunds, and guest engagement.

This project analyzes 36,275 bookings, builds and compares Logistic Regression and Decision Tree models, and delivers actionable recommendations for cancellation policy and revenue protection.

---

## Dataset

| Feature | Description |
|---|---|
| `no_of_adults` / `no_of_children` | Guest composition |
| `no_of_weekend_nights` / `no_of_week_nights` | Stay duration |
| `type_of_meal_plan` | Meal plan selected at booking |
| `required_car_parking_space` | Parking requirement (0/1) |
| `room_type_reserved` | Encoded room type (1–7) |
| `lead_time` | Days between booking and arrival |
| `arrival_year` / `arrival_month` / `arrival_date` | Arrival date components |
| `market_segment_type` | Booking channel (Online, Offline, Corporate, Aviation, Complementary) |
| `repeated_guest` | Repeat guest flag (0/1) |
| `no_of_previous_cancellations` | Prior cancellation history |
| `no_of_previous_bookings_not_canceled` | Prior completed bookings |
| `avg_price_per_room` | Dynamic room price in EUR |
| `no_of_special_requests` | Number of special requests made |
| `booking_status` | **Target variable** — Canceled or Not Canceled |

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
| Data cleaning | Removed 78 zero-night/zero-price records identified as erroneous entries |
| Feature engineering | Created `total_nights`, `cancellation_rate`, `total_previous_bookings` for EDA; dropped before modeling to prevent multicollinearity |
| Encoding | One-hot encoding with `drop_first=True`; binary target encoding |
| Logistic Regression | Statsmodels Logit with automated backward elimination; threshold optimization via ROC and Precision-Recall curves |
| Decision Tree | Unpruned baseline → pre-pruning via GridSearchCV → post-pruning via cost-complexity path |
| Model selection | F1 score as primary metric, balancing precision and recall per business objective |

---

## Model Performance Summary

| Model | Accuracy | Recall | Precision | F1 | ROC-AUC |
|---|---|---|---|---|---|
| Logistic Regression (0.439 threshold) — Test | ~0.7969 | ~0.6839 | ~0.6932 | ~0.6885 | ~0.8576 |
| Decision Tree Unpruned — Test | ~0.8647 | ~0.8020 | ~0.7893 | ~0.7956 | ~0.8514 |
| Decision Tree Pre-pruned — Test | ~0.8710 | ~0.7669 | ~0.8275 | ~0.7960 | ~0.9280 |
| **Decision Tree Post-pruned — Test** | **~0.8738** | **~0.7843** | **~0.8231** | **~0.8032** | **~0.9296** |

**Selected model: Post-pruned Decision Tree** — highest F1 test data with a train/test F1 gap of only 0.0474, indicating strong generalization.

---

## Key Findings

- **Lead time is the strongest cancellation predictor** — each additional day between booking and arrival increases cancellation odds by ~2%, meaning a booking made 100 days out has roughly twice the cancellation risk of a same-day booking.
- **Special requests are a strong cancellation inverse signal** — each additional special request reduces cancellation odds by 78%. Whether this reflects guest commitment (signal) or engagement (driver) is an open question worth testing.
- **Repeat guests cancel at 2% vs. 34% for new guests** — repeat guest status is the most powerful single protective factor in the logistic regression.
- **Offline bookings cancel 84% less than online** — suggesting that friction in the booking process correlates with higher commitment.
- **July has the highest monthly cancellation rate at 45%**, despite being outside the peak booking months of August–October.
- **Decision trees substantially outperform logistic regression** across all metrics, suggesting the relationships likely include non-linear components.

---

## Business Recommendations

- **Implement the post-pruned decision tree for real-time cancellation prediction** — use model outputs to drive dynamic overbooking decisions proportional to predicted cancellation rates by booking segment.
- **Apply stricter cancellation policies or non-refundable incentives for high lead-time bookings** — the model's strongest predictor directly informs where policy friction has the highest ROI.
- **A/B test a prompted special requests workflow** — guests who make special requests cancel far less, but causality is ambiguous. A controlled test would determine whether proactively prompting for preferences at booking reduces cancellations, before committing to an operational change.
- **Investigate repeat guest attrition** — with only 2.6% repeat guests and a 2% cancellation rate among them, repeat guests are both rare and extremely reliable. Understanding what prevents return visits is a high-value research question.
- **Tighten overbooking guardrails in August–October** — these months have the highest booking volume; overbooking errors carry the greatest operational cost during peak periods.

---

## Notable Implementation Details

- Identified and removed 78 records simultaneously showing zero-night stays, zero room revenue, and a completed booking status — likely erroneous entries from a separate business process, not standard reservations.
- Identified the `arrival_year` variable as unreliable for modeling due to incomplete 2017 data coverage (second half only), and dropped it with documented reasoning.
- Recognized the ambiguity in the special requests cancellation signal and designed a specific A/B test recommendation to resolve causality before acting on it operationally.
- Built reusable utility functions for distribution analysis, correlation filtering, VIF computation, model evaluation, and backward elimination — keeping the notebook clean and methodology reproducible.

---

## Skills Demonstrated

- Binary classification (Logistic Regression, Decision Tree)
- Hyperparameter tuning via GridSearchCV with cross-validation
- Cost-complexity post-pruning with alpha path optimization
- Threshold optimization via ROC and Precision-Recall curves
- Feature engineering and motivated variable dropping
- Multicollinearity analysis (VIF)
- Model comparison and business-driven model selection
- Causal reasoning and experimental design recommendations

---

*This project was completed as part of the Post Graduate Program in Data Science with Generative AI: Applications to Business through the University of Texas at Austin McCombs School of Business. Full code written independently from scratch using the full-code submission path rather than the provided low-code template.*
