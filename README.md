# Part 2: KPI Framework, Business Experiment Analysis & Decision Recommendation

## 1. Business Context

A subscription-based digital product company launched a new onboarding and activation campaign. Users were randomly divided into two groups:
- **Control group (n=600):** Existing onboarding experience
- **Treatment group (n=600):** New campaign experience

The business question: **Should the new campaign be launched to all users?**

---

## 2. Dataset Description

**File:** `data/campaign_experiment_data.xlsx`

| Column | Description |
|---|---|
| user_id | Unique user identifier |
| group | Control or Treatment |
| region | North / South / East / West |
| device_type | Mobile / Desktop / Tablet |
| traffic_source | Organic / Paid / Referral / Email |
| plan_type | Basic / Pro / Enterprise |
| visited_landing_page | Binary (0/1) |
| started_trial | Binary (0/1) |
| completed_onboarding | Binary (0/1) |
| converted_to_paid | Binary (0/1) — **Primary metric** |
| revenue | Revenue generated (0 if not converted) |
| refund_requested | Binary (0/1) — **Guardrail metric** |
| support_ticket | Binary (0/1) — **Guardrail metric** |
| engagement_score | 0–100 score — **Guardrail metric** |
| days_to_convert | Days from start to conversion (NULL if not converted) |

**Data quality issues identified and handled:**
- 1 duplicate `user_id` (U00100 appeared twice) — duplicate removed
- 6 rows with NULL `region` — filled as "Unknown"
- 2 rows with negative `revenue` (-$50) — flagged and excluded from revenue summaries

---

## 3. North Star Metric Selected

**Paid Conversion Rate** — the proportion of users who converted to a paid subscription plan.

This metric directly measures whether the campaign achieves its business goal (revenue growth). All other metrics (trial start rate, onboarding completion, engagement score) are drivers or guardrails, not the end outcome.

---

## 4. KPI Tree Summary

```
NORTH STAR: Paid Conversion Rate
├── Driver 1: Awareness & Reach
│   ├── Landing Page Visit Rate
│   ├── Traffic Source Quality
│   └── Campaign Reach
├── Driver 2: Activation & Engagement
│   ├── Trial Start Rate
│   ├── Onboarding Completion Rate
│   └── Engagement Score
├── Driver 3: Revenue Quality
│   ├── Avg Revenue Per Converted User
│   ├── Plan Type Distribution
│   └── Days to Convert
└── Guardrail Metrics
    ├── Refund Rate (threshold < 12%)
    ├── Support Ticket Rate (threshold < 18%)
    └── Segment-level Conversion Stability
```

See `outputs/kpi_tree.png` for the visual KPI tree.

---

## 5. Experiment Analysis Approach

**File:** `analysis/experiment_analysis.xlsx`

Sheets included:
1. **Data Quality Check** — duplicate check, null check, invalid revenue check, binary validation, segment balance verification
2. **Experiment Summary** — all 11 required metrics compared Control vs Treatment with absolute lift and % change
3. **Segment — Region** — conversion rate by region for both groups
4. **Segment — Device** — conversion rate by device type
5. **Segment — Traffic Source** — average revenue per user by traffic source

---

## 6. Hypothesis Test Summary

**Test:** One-tailed Z-test for proportions on Paid Conversion Rate

| Parameter | Value |
|---|---|
| H₀ | p_treatment ≤ p_control |
| H₁ | p_treatment > p_control |
| α | 0.05 |
| Z-Statistic | ~2.40 |
| P-Value (one-tailed) | ~0.0082 |
| Decision | **REJECT H₀** |

The improvement in conversion rate is statistically significant (p < 0.01).

Full test details: `analysis/hypothesis_test_notes.md`
Test output evidence: `screenshots/hypothesis_test_output.png`

---

## 7. Guardrail Metrics Considered

| Guardrail | Control | Treatment | Change | Assessment |
|---|---|---|---|---|
| Refund Rate | ~4.5% | ~5.5% | +1 pp | ✅ Below threshold |
| Support Ticket Rate | ~12% | ~13% | +1 pp | ✅ Below threshold |
| Avg Days to Convert | ~14.2 | ~13.8 | -0.4 days | ✅ Improved |
| Avg Engagement Score | ~62 | ~67 | +5 pts | ✅ Improved |
| Avg Rev Per Converted User | ~$98 | ~$101 | +$3 | ✅ Revenue quality maintained |

No guardrail metric breached its threshold.

---

## 8. Final Recommendation

> **✅ LAUNCH the campaign to all users.**

- Paid conversion rate improved significantly (+4.5 pp, p < 0.01)
- All guardrail metrics within acceptable limits
- Consistent positive lift across all regions
- Revenue quality maintained

Launch conditions: Monitor refund and support ticket rates daily for 7 days post-launch. Alert threshold: refund > 10% or support tickets > 20%.

Full recommendation: `outputs/recommendation_memo.md`

---

## 9. Assumptions and Limitations

- Dataset is representative/synthetic for this assignment — real data may show different variance.
- No pre-experiment A/A test was conducted; group balance was verified but not statistically proven equivalent.
- Seasonal or temporal confounds were not controlled for.
- The test assumes random assignment was properly implemented (not independently verified).
- Enterprise-tier users may behave differently; the campaign should be separately evaluated for enterprise before mass launch.

---

## 10. Screenshots Included

| File | Content |
|---|---|
| `screenshots/summary_metrics.png` | Control vs Treatment summary comparison table |
| `screenshots/hypothesis_test_output.png` | Z-test setup, output, and decision |
| `screenshots/kpi_tree_preview.png` | KPI tree diagram |

---

## Repository Structure

```
part2_kpi_experiment/
├── data/
│   └── campaign_experiment_data.xlsx
├── analysis/
│   ├── experiment_analysis.xlsx
│   └── hypothesis_test_notes.md
├── outputs/
│   ├── kpi_tree.png
│   ├── experiment_summary.xlsx
│   └── recommendation_memo.md
├── screenshots/
│   ├── summary_metrics.png
│   ├── hypothesis_test_output.png
│   └── kpi_tree_preview.png
└── README.md
```

**Tools used:** Python (openpyxl, math), Excel (for manual verification), Markdown

**Student:** Ejaz Ali Khan
**Assignment:** Business Analytics Capstone — Part 2
