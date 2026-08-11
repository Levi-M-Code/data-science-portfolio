# Trade&Ahead — Stock Clustering with Unsupervised Learning

## Business Problem

Trade&Ahead, a financial consultancy firm, needed a data-driven way to help clients build diversified investment portfolios without getting lost in the complexity of analyzing hundreds of individual stocks across dozens of financial metrics. By grouping stocks that exhibit similar financial characteristics, investors can more efficiently identify diversification opportunities and protect against correlated risk.

This project analyzes 340 NYSE-listed stocks across 11 GICS sectors, builds and compares K-Means and Hierarchical clustering models on both capped and uncapped data, and delivers actionable portfolio construction recommendations based on cluster characteristics.

---

## Dataset

| Feature | Description |
|---|---|
| `Ticker Symbol` | Stock ticker abbreviation |
| `Security` | Company name |
| `GICS Sector` | Economic sector (Global Industry Classification Standard) |
| `GICS Sub Industry` | Sub-industry classification |
| `Current Price` | Stock price in USD |
| `Price Change` | 13-week percentage price change |
| `Volatility` | Standard deviation of price over 13 weeks |
| `ROE` | Return on Equity (%) |
| `Cash Ratio` | Cash and equivalents / current liabilities |
| `Net Cash Flow` | Cash inflows minus outflows (USD) |
| `Net Income` | Revenue minus expenses, interest, and taxes (USD) |
| `Earnings Per Share` | Net profit / shares outstanding (USD) |
| `Estimated Shares Outstanding` | Total shares held by all shareholders |
| `P/E Ratio` | Stock price / earnings per share |
| `P/B Ratio` | Stock price / book value per share |

**Source:** Course-provided dataset, Post Graduate Program in Data Science with Generative AI: Applications to Business — University of Texas at Austin McCombs School of Business

---

## Tools Used

- Python (pandas, numpy, scikit-learn, scipy, yellowbrick)
- Matplotlib, Seaborn
- Jupyter Notebook

---

## Methodology

| Step | Approach |
|---|---|
| Outlier treatment | Built both capped (IQR factor 3.0 on P/E, ROE, P/B) and uncapped datasets; ran all models on both to compare sensitivity |
| Scaling | StandardScaler applied to all numerical features before clustering |
| K-Means k selection | Elbow method (KElbowVisualizer) + silhouette scores + cluster size balance checks |
| Hierarchical linkage selection | Cophenetic correlation tested across 4 distance metrics × 7 linkage methods; euclidean/ward selected |
| Hierarchical k selection | Dendrogram visual inspection + silhouette scores + cluster size balance checks |
| Cross-model comparison | Custom overlap analysis tracking company consistency across all 4 models; price change and volatility comparison across equivalent clusters |

---

## Models Built

| Model | k | Cophenetic / Silhouette |
|---|---|---|
| K-Means Uncapped | 5 | Silhouette: 0.4382 (k=5) |
| K-Means Capped | 5 | Silhouette: 0.3656 (k=5) |
| Hierarchical Uncapped (Ward) | 4 | Cophenetic: 0.710 |
| Hierarchical Capped (Ward) | 4 | Cophenetic: 0.670 |

**Selected model: K-Means Capped (k=5)** — highest price gain in the premium cluster, lowest volatility in the core market cluster, and lower computational cost than the comparable hierarchical models.

---

## Cluster Summary (Selected Model — K-Means Capped)

| Cluster | Label | Size | Key Characteristics |
|---|---|---|---|
| 0 | Premium Growth | 30 | Highest price, strongest gains, high cash ratio, high EPS |
| 1 | Highly Distressed | 6 | Very low price, severe losses, high volatility, mostly Energy |
| 2 | Distressed | 25 | Low price, negative income, volatile, mostly Energy |
| 3 | Large & Established | 14 | Household names, highest net income, lowest volatility, negative P/B |
| 4 | Core Market | 265 | Moderate across all metrics, all sectors represented |

---

## Key Findings

- **All four models (two K-Means, two Hierarchical, capped and uncapped) produced remarkably consistent cluster structures** — 260 of 340 companies appeared in the same "Core Market" cluster across all four models, and 9 of 14 "Large & Established" companies appeared consistently across all models.
- **Capping outliers had minimal practical impact** — cluster meanings and company assignments were highly similar between capped and uncapped versions of both algorithms, suggesting the cluster structure is robust to extreme values.
- **Energy sector is a persistent risk signal** — distressed clusters were 70–86% Energy sector companies across every model, making sector-level avoidance a defensible default for conservative portfolios.
- **Healthcare had the strongest 13-week price performance** (mean: 9.59%, median: 10.32%) while Energy had the most negative (mean: -9.24%).
- **Negative P/B ratio is not a red flag in isolation** — the Large & Established cluster (household-name companies with the highest net income) all showed negative book value, consistent with heavy share buybacks and leverage rather than financial distress.
- **ROE interpretation requires context** — the most distressed cluster showed the highest ROE, a known ratio artifact that occurs when both net income and shareholder equity are negative.

---

## Portfolio Recommendations

- **Core portfolios should draw from the Core Market (Cluster 4) and Large & Established (Cluster 3)** — best balance of steady gains and low volatility.
- **Conservative portfolios needing near-term liquidity** should weight heavily toward Cluster 3 (lowest volatility, above-average gains).
- **Aggressive investors with longer horizons** should consider shifting allocation toward the Premium Growth cluster (Cluster 0) — higher gain potential with moderate-to-high volatility.
- **Avoid the Energy sector** as a default unless the client has sector expertise and high risk tolerance — it dominates every distressed cluster across every model.
- **Sector-level ETF targeting:** Consumer Staples (high gains, very low volatility), Healthcare (highest gains, moderate volatility), and Financials (moderate gains, low volatility) stand out as favorable sector exposures.

---

## Notable Implementation Details

- Ran cophenetic correlation analysis across 19 distance metric and linkage method combinations to select the optimal hierarchical clustering configuration rather than defaulting to a single method.
- Investigated k=2 silhouette score dominance in the uncapped K-Means data — checked actual cluster sizes to test whether it reflected outlier isolation vs. genuine structure, and documented the finding that weakened (but did not eliminate) the case against k=2.
- Identified reusable utility functions for cluster overlap analysis and cross-model comparison — conceptual design and analytical logic are original; implementation benefited from AI coding assistance.
- Built a cross-model comparison framework that tracks individual company consistency across all four models, enabling confidence-weighted cluster interpretation rather than relying on any single algorithm's output.

---

## Skills Demonstrated

- Unsupervised learning (K-Means, Agglomerative Hierarchical Clustering)
- Elbow method and silhouette analysis for cluster count selection
- Cophenetic correlation for hierarchical linkage method selection
- Outlier sensitivity analysis (capped vs. uncapped comparison)
- Cross-model validation and cluster overlap analysis
- Financial metric interpretation in a business context
- Portfolio construction recommendations from cluster analysis
- Custom utility function design for reusable analytical workflows

---

*This project was completed as part of the Post Graduate Program in Data Science with Generative AI: Applications to Business through the University of Texas at Austin McCombs School of Business. Full code written independently from scratch using the full-code submission path rather than the provided low-code template.*
