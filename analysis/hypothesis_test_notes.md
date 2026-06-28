# Hypothesis Test Notes — Part 2: KPI Experiment Analysis

## Business Context
The company launched a new onboarding and activation campaign for a subscription-based digital product.
Users were split into two groups:
- **Control group (n=600):** Existing onboarding experience
- **Treatment group (n=600):** New campaign experience

The core business question: **Does the new campaign meaningfully improve user conversion to paid plans?**

---

## Metric Selected for Testing

**Paid Conversion Rate** — the proportion of users in each group who converted to a paid subscription.

**Why this metric?**
- It is directly linked to revenue and business growth.
- It is the North Star metric for this experiment (see `recommendation_memo.md`).
- Other metrics (trial start rate, onboarding completion) are intermediate; conversion is the final proof.
- It is a binary outcome (converted / not converted), making a proportion Z-test the correct statistical approach.

---

## Hypotheses

| | Statement |
|---|---|
| **Null Hypothesis (H₀)** | The paid conversion rate for the Treatment group is less than or equal to the Control group's conversion rate. (p_treatment ≤ p_control) |
| **Alternate Hypothesis (H₁)** | The paid conversion rate for the Treatment group is greater than the Control group's conversion rate. (p_treatment > p_control) |

---

## Test Design

| Parameter | Value | Reason |
|---|---|---|
| Test type | One-tailed Z-test for proportions | We only care about *improvement*, not bidirectional change |
| Significance level (α) | 0.05 | Industry standard; balances Type I and Type II error risk |
| Metric tested | Paid Conversion Rate | Primary North Star metric |
| Control group size (n₁) | 600 | Balanced split |
| Treatment group size (n₂) | 600 | Balanced split |

---

## Test Inputs & Calculation

| Input | Control | Treatment |
|---|---|---|
| Users | 600 | 600 |
| Converters | ~114 | ~141 |
| Conversion Rate | ~19.0% | ~23.5% |
| Observed Lift | — | +4.5 pp |

**Pooled proportion (p_pool):**
```
p_pool = (converters_control + converters_treatment) / (n_control + n_treatment)
```

**Standard Error:**
```
SE = sqrt(p_pool × (1 - p_pool) × (1/n₁ + 1/n₂))
```

**Z-Statistic:**
```
Z = (p_treatment - p_control) / SE
```

**P-Value (one-tailed):**
```
p_value = 1 - Φ(Z)    where Φ is the standard normal CDF
```

---

## Test Output (see `outputs/experiment_summary.xlsx` → Hypothesis Test sheet)

| Stat | Value |
|---|---|
| Z-Statistic | ~2.40 |
| P-Value (two-tailed) | ~0.0163 |
| **P-Value (one-tailed)** | **~0.0082** |
| Significance level (α) | 0.05 |
| **Decision** | **REJECT H₀** |

---

## Decision Rule

> **If p-value (one-tailed) < 0.05 → Reject H₀ → Treatment shows statistically significant improvement.**

Since our one-tailed p-value (~0.0082) is **well below 0.05**, we reject the null hypothesis.

---

## Business Interpretation

The new onboarding and activation campaign produced a **statistically significant improvement** in paid conversion rate (~+4.5 percentage points). This is not due to random chance at the 95% confidence level.

However, statistical significance alone is not sufficient to recommend launch. The guardrail metrics must also be evaluated (see `recommendation_memo.md`):
- Refund rate increased marginally (+1 pp) — within acceptable thresholds
- Support ticket rate increased marginally — likely a short-term learning curve effect
- Average days to convert improved — users converted faster under the new experience
- Engagement score improved — users were more actively engaged

**Combined interpretation:** The evidence supports launching the new campaign to all users, with close monitoring of refund and support metrics in the first week post-launch.

---

## Limitations

- The dataset is synthetic/representative; real-world variance may differ.
- A/A test was not run prior to this A/B test — pre-experiment equivalence was assumed but not statistically proven.
- The test does not control for temporal confounds (e.g., seasonal traffic effects).
- Sample size was fixed at 600 per group — power analysis was not formally documented pre-experiment.
- The Z-test assumes large-sample normal approximation; a chi-square test of independence would yield equivalent results and could serve as a cross-check.
