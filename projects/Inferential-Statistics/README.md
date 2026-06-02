# E-News Express — A/B Testing & Inferential Statistics

## Business Problem

E-News Express, an online news portal, observed a potential decline in new monthly subscribers and hypothesized that their existing landing page was underperforming in terms of design and content relevance. The design team developed a new landing page and tasked the data science team with running a controlled experiment to determine whether the new page drives measurably better user engagement and conversion.

This project analyzes the results of an A/B test conducted on 100 users (50 control, 50 treatment) using statistical hypothesis testing at a 5% significance level to answer four key business questions.

---

## Dataset

| Feature | Description |
|---|---|
| `user_id` | Unique user identifier |
| `group` | Control (old page) or Treatment (new page) |
| `landing_page` | Old or new landing page served |
| `time_spent_on_the_page` | Time (in minutes) spent on the landing page |
| `converted` | Whether the user subscribed (yes/no) |
| `language_preferred` | Language selected by the user (English, French, Spanish) |

**Source:** Course-provided dataset, Post Graduate Program in Data Science with Generative AI: Applications to Business — University of Texas at Austin McCombs School of Business

---

## Tools Used

- Python (pandas, numpy, scipy, statsmodels)
- Matplotlib, Seaborn
- Jupyter Notebook

---

## Hypothesis Tests Conducted

| Question | Test Used | Result |
|---|---|---|
| Do users spend more time on the new page? | Two-sample independent t-test (Welch's) | Reject H₀ — users spend significantly more time on new page |
| Is conversion rate higher on the new page? | Two-proportion z-test | Reject H₀ — new page conversion rate is significantly higher |
| Does conversion depend on language preference? | Chi-square test for independence | Fail to reject H₀ — no significant association found |
| Is time spent on new page the same across languages? | One-way ANOVA | Fail to reject H₀ — no significant difference across languages |

---

## Key Findings

- **Users spent significantly more time on the new landing page** — 6.22 minutes on average vs. 4.53 minutes on the old page.
- **The new page drove a 24-point lift in conversion rate** — 66% vs. 42% on the old page, a statistically significant improvement.
- **Language preference does not significantly affect conversion or time spent** — the new page performs consistently across English, French, and Spanish users, simplifying the rollout decision.
- **Assumption validation was performed before each test** — Levene's test for equal variance and Shapiro-Wilk for normality, ensuring appropriate test selection throughout.

---

## Business Recommendations

1. **Implement the new landing page as the default experience** — both engagement and conversion improvements are statistically significant and practically meaningful.
2. **Identify and leverage high-performing page elements** — analyze layout, content relevance, and call-to-action placement to understand what is driving the improvement and apply those learnings to future iterations.
3. **Prioritize engagement as a conversion driver** — users who spend more time on the page convert at a higher rate; interactive or personalized content could further extend time on page.
4. **Maintain a consistent experience across languages** — no language-specific redesigns are warranted based on current data, reducing implementation complexity.
5. **Re-evaluate language performance at scale** — while current differences are not statistically significant, the sample size of 100 users is small; continued monitoring with larger samples is recommended.

---

## Skills Demonstrated

- A/B test design and validation
- Hypothesis testing (t-test, z-test, chi-square, ANOVA)
- Statistical assumption validation (Levene's test, Shapiro-Wilk)
- Experimental analysis and inference
- Business recommendation from statistical findings
- Data visualization (boxplots, histograms, countplots)

---

*This project was completed as part of the Post Graduate Program in Data Science with Generative AI: Applications to Business through the University of Texas at Austin McCombs School of Business. Full code written independently from scratch using the full-code submission path rather than the provided low-code template.*
