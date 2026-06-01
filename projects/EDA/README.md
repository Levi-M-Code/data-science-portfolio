# FoodHub Order Analysis — Exploratory Data Analysis

## Business Problem

FoodHub is a New York-based food aggregator operating a multi-restaurant delivery platform. As order volume grows, the company needs a clearer understanding of customer behavior, restaurant performance, and operational efficiency to improve the customer experience and drive revenue.

This project analyzes 1,898 orders across 178 restaurants to answer key business questions and deliver actionable recommendations.

---

## Dataset

| Feature | Description |
|---|---|
| `order_id` | Unique order identifier |
| `customer_id` | Unique customer identifier |
| `restaurant_name` | Restaurant fulfilling the order |
| `cuisine_type` | Type of cuisine ordered |
| `cost_of_the_order` | Order cost in USD |
| `day_of_the_week` | Weekday or Weekend |
| `rating` | Customer rating out of 5 (or "Not given") |
| `food_preparation_time` | Minutes from order confirmation to pickup |
| `delivery_time` | Minutes from pickup to delivery |

**Source:** Course-provided dataset, Post Graduate Program in Data Science with Generative AI: Applications to Business — University of Texas at Austin McCombs School of Business

---

## Tools Used

- Python (pandas, numpy)
- Matplotlib, Seaborn
- Jupyter Notebook

---

## Key Findings

- **39% of orders were unrated** — no rating below 3 was ever given, suggesting customers may opt out rather than rate poorly. A potential app-level bug preventing low ratings warrants investigation.
- **Weekends drive 71% of all orders** and weekday deliveries run an average of 6 minutes slower, likely due to traffic — a meaningful operational lever.
- **22 restaurants (12%) were never rated** — despite comparable delivery times and order costs, these restaurants had almost no repeat customers, suggesting quality issues rather than service failures.
- **Number of orders drops at around the 30 min delivery time** with the outlier fence at ~40 min.

---

## Business Recommendations

- **Build a recommendation algorithm** using cuisine affinity and repeat ordering behavior — most customers don't reorder from the same restaurant, but do reorder the same cuisine type, suggesting variety-within-preference is the right targeting strategy.
- **Deprioritize slow >30 min delivery in the algorithm** — real-time routing data combined with a 30-minute delivery threshold could meaningfully reduce the 10.5% of orders exceeding 60 minutes total and drive repeat app use.
- **Target weekday lunch growth** with time-sensitive push notifications leveraging location data and predicted delivery windows — weekday orders represent significant untapped volume.
- **Base restaurant promotional eligibility on both rating and rating count** — many restaurants have too few ratings to be statistically reliable.
- **Investigate the ratings floor** — the absence of any rating below 3 is statistically unlikely in a genuine distribution and warrants a technical audit of the rating submission flow.

---

## Skills Demonstrated

- Exploratory Data Analysis (univariate and multivariate)
- Data cleaning and feature engineering
- Statistical summarization and outlier detection
- Business insight generation from operational data
- Data visualization (histograms, boxplots, heatmaps, pairplots, regression plots)

---

*This project was completed as part of the Post Graduate Program in Data Science with Generative AI: Applications to Business through the University of Texas at Austin McCombs School of Business. Full code written independently from scratch using the full-code submission path rather than the provided low-code template.*
