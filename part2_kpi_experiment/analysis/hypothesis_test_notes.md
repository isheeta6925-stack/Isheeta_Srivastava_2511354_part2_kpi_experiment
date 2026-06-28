# Hypothesis Test Notes
## Campaign Experiment Analysis – Part 2 (KPI Framework & Experiment Analysis)

---

## Business Context

The company ran an A/B experiment to determine whether a new onboarding and activation campaign
improves user conversion and engagement. Users were randomly split into:

- **Control Group (n = 693):** Existing onboarding experience
- **Treatment Group (n = 715):** New campaign experience

The core business question is: **Should the new campaign be launched to all users?**

---

## Test 1: Primary Hypothesis – Paid Conversion Rate

### Metric Being Tested
**Paid Conversion Rate** = Users converted to paid / Total users in group

This is the **North Star Metric** because it directly measures the campaign's business impact.

### Hypotheses

| | Statement |
|---|---|
| **Null Hypothesis (H₀)** | The paid conversion rate is equal between Control and Treatment groups. There is no statistically significant difference. |
| **Alternate Hypothesis (H₁)** | The paid conversion rate is different between Control and Treatment groups. |

### Test Configuration

| Parameter | Value |
|---|---|
| Test Type | Chi-Square Test of Independence |
| Tails | Two-tailed (we test for any difference, not just improvement) |
| Significance Level (α) | 0.05 |
| Degrees of Freedom | 1 |

### Reason for Choosing This Test
The paid conversion rate is a **binary outcome** (each user either converts or doesn't).
Chi-Square is the correct test for comparing proportions between two independent groups.
A t-test would be inappropriate here since the underlying distribution is Bernoulli, not Normal.

### Contingency Table (Inputs)

| Group | Converted | Not Converted | Total |
|---|---|---|---|
| Control | 22 | 671 | 693 |
| Treatment | 50 | 665 | 715 |
| **Total** | **72** | **1,336** | **1,408** |

### Test Output

| Statistic | Value |
|---|---|
| Chi-Square Statistic | **9.8024** |
| Degrees of Freedom | 1 |
| P-Value | **0.001743** |
| Alpha | 0.05 |

### Decision Rule
- If p-value < 0.05 → **Reject H₀**
- If p-value ≥ 0.05 → **Fail to reject H₀**

### Result
**P-value = 0.0017 < 0.05 → REJECT H₀**

### Business Interpretation
There is **statistically significant evidence** that the Treatment campaign improves paid conversion rate.
Treatment group converted at **6.99%** vs Control's **3.17%** — a **+3.82 percentage point lift**,
which represents a **~120% relative improvement** in conversion.

This finding is unlikely to be due to random chance (probability of observing this difference by
chance is only 0.17%).

---

## Test 2: Secondary Hypothesis – Engagement Score

### Metric Being Tested
**Average Engagement Score** – Continuous score measuring user engagement depth.

### Hypotheses

| | Statement |
|---|---|
| **Null Hypothesis (H₀)** | Mean engagement score is equal in Control and Treatment groups. |
| **Alternate Hypothesis (H₁)** | Mean engagement score differs between Control and Treatment groups. |

### Test Configuration

| Parameter | Value |
|---|---|
| Test Type | Independent Samples t-test (Welch's t-test) |
| Tails | Two-tailed |
| Significance Level (α) | 0.05 |

### Reason for Choosing This Test
Engagement score is a **continuous numeric variable**. An independent samples t-test is appropriate
for comparing means between two independent groups. With n > 600 per group, the Central Limit
Theorem ensures normality of the sampling distribution.

### Test Output

| Statistic | Value |
|---|---|
| Control Mean | 57.03 |
| Treatment Mean | 62.93 |
| Difference | +5.90 |
| t-Statistic | **7.9445** |
| P-Value | **< 0.0001** |
| Alpha | 0.05 |

### Decision Rule
P-value < 0.0001 < 0.05 → **REJECT H₀**

### Business Interpretation
Treatment users are **significantly more engaged** than control users. This is a strong supporting
signal confirming that the campaign creates genuine product value, not just surface-level conversions.

---

## Interpretation Logic Summary

| Scenario | Interpretation |
|---|---|
| p < α AND business lift is positive | Reject H₀ – campaign works, consider launch |
| p < α BUT guardrail metrics are damaged | Reject H₀ – but investigate before full launch |
| p ≥ α | Fail to reject H₀ – campaign not proven to work |

**Our finding:** p < α for both primary and secondary tests, with positive business lift.
Guardrail review (see experiment_summary.xlsx) shows some risk in support ticket rate and
revenue-per-converted-user that must be addressed before full rollout.

---

## Connection to Business Decision

The hypothesis tests provide the **statistical foundation** for the launch recommendation.
The combination of:
1. Significantly higher paid conversion rate (p = 0.0017)
2. Significantly higher engagement score (p < 0.0001)
3. Faster time to conversion (-2.46 days on average)

…supports a **conditional launch recommendation** with guardrail monitoring in place.
