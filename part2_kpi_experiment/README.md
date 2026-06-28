# Part 2: KPI Framework, Business Experiment Analysis & Decision Recommendation

## 1. Business Context

A subscription-based digital product company launched a new **onboarding and activation campaign**
to improve user conversion and early engagement. Users were randomly split into:

- **Control Group (n = 693):** Existing onboarding experience
- **Treatment Group (n = 715):** New campaign experience

Leadership must decide whether to **launch the new campaign to all users**, and this analysis
provides the data-driven evidence required to make that decision.

**Business Problem Statement:**
The company needs to determine whether the new onboarding campaign meaningfully improves paid
conversion rate — the key revenue driver — without damaging retention quality, support costs,
or refund rates. The decision impacts all new user acquisition (estimated thousands of users
per month), and the evidence required is a statistically significant lift in conversion with
acceptable guardrail metric performance.

---

## 2. Dataset Description

**File:** `data/campaign_experiment_data.xlsx`  
**Rows:** 1,408 users (693 Control, 715 Treatment)  
**Columns:** 16

| Column | Description |
|---|---|
| user_id | Unique user identifier |
| signup_date | Date of signup |
| experiment_group | Control or Treatment |
| region | North / South / East / West |
| device_type | Desktop / Mobile / Tablet |
| traffic_source | Email / Organic / Paid Search / Referral / Social |
| plan_type | Free / Basic / Premium |
| visited_landing_page | Binary: 1 = visited |
| started_trial | Binary: 1 = started trial |
| completed_onboarding | Binary: 1 = completed onboarding |
| converted_to_paid | Binary: 1 = converted (North Star outcome) |
| revenue_30d | 30-day revenue for the user ($) |
| support_tickets_30d | Number of support tickets raised |
| refund_requested | Binary: 1 = requested refund |
| days_to_convert | Days from signup to paid conversion (blank if not converted) |
| engagement_score | Numeric engagement depth score |

---

## 3. North Star Metric Selected

**Paid Conversion Rate** = Users converted to paid / Total users in group

| Group | Converted | Total | Rate |
|---|---|---|---|
| Control | 22 | 693 | 3.17% |
| Treatment | 50 | 715 | 6.99% |
| **Lift** | **+28** | — | **+3.82 pp (+120% relative)** |

This metric was selected because it:
- Directly drives subscription revenue
- Is unambiguous (binary outcome, easy to measure)
- Connects all funnel stages into a single outcome
- Can be tested with appropriate statistical methods (Chi-Square)

---

## 4. KPI Tree Summary

The full KPI tree is in `outputs/kpi_tree.png`.

```
NORTH STAR: Paid Conversion Rate
├── Acquisition & Top-of-Funnel
│   ├── Landing Page Visit Rate
│   ├── Trial Start Rate
│   ├── Traffic Source Mix
│   └── Device/Region Reach
├── Activation & Onboarding Quality
│   ├── Onboarding Completion Rate
│   ├── Days to Convert
│   ├── UX Flow Quality
│   └── Support Ticket Rate [Guardrail]
└── Engagement & Retention Value
    ├── Engagement Score
    ├── Avg Revenue per Converted User [Guardrail]
    ├── Plan Upgrade Rate
    └── Refund Rate [Guardrail]
```

---

## 5. Experiment Analysis Approach

**Data cleaning steps performed (see `analysis/experiment_analysis.xlsx` → 'Data Quality Checks' sheet):**
- Verified no duplicate user_ids (result: 0 duplicates)
- Checked group counts: 693 Control, 715 Treatment — balanced split
- Identified 18 missing `device_type` values and 24 missing `traffic_source` values — retained in main analysis, excluded from segment-specific analysis
- Identified 14 missing `engagement_score` values — excluded from mean calculations
- 1,336 missing `days_to_convert` values — expected, as only the 72 converted users have a conversion date
- Revenue outlier check: max value $8,610.72 is consistent with Premium plan pricing — retained
- Binary columns (converted_to_paid, refund_requested, etc.) validated — all values are 0 or 1
- No overwriting of raw data — all analysis performed on separate sheets

---

## 6. Hypothesis Test Summary

**Primary Test: Paid Conversion Rate (Chi-Square)**

| | Value |
|---|---|
| H₀ | Conversion rate equal in both groups |
| H₁ | Conversion rate differs between groups |
| Test | Chi-Square Test of Independence |
| Chi-Square Statistic | 9.8024 |
| P-Value | **0.001743** |
| Alpha | 0.05 |
| **Decision** | **REJECT H₀ — Statistically significant improvement** |

**Secondary Test: Engagement Score (t-test)**
- t-statistic = 7.9445, p < 0.0001 → Reject H₀ (engagement significantly higher in Treatment)

---

## 7. Guardrail Metrics Considered

| Guardrail Metric | Control | Treatment | Risk |
|---|---|---|---|
| Refund Rate | 0.00% | 0.42% | LOW |
| Support Ticket Rate | 14.72% | 24.76% | MEDIUM ⚠ |
| Avg Revenue per Converted User | $1,630 | $770 | MEDIUM ⚠ |
| Days to Convert | 8.86 days | 6.40 days | LOW (positive) |
| Engagement Score | 57.03 | 62.93 | LOW (positive) |

The elevated support ticket rate in Treatment is the key guardrail concern.
A UX audit of the new onboarding flow is recommended before or alongside full launch.

---

## 8. Final Recommendation

**LAUNCH the new campaign to all users.**

Statistical evidence is strong (p = 0.0017 for conversion rate; p < 0.0001 for engagement).
All regions and device types benefit. The campaign is particularly effective for Free plan users
and Referral traffic.

Launch should be accompanied by:
- UX audit to reduce support ticket friction
- 30-day guardrail monitoring dashboard
- 60-day LTV cohort analysis to validate revenue quality

---

## 9. Assumptions and Limitations

- Randomisation is assumed to be valid — no pre-experiment baseline comparison was available
- 30-day revenue window may not reflect true LTV (long-term churn unknown)
- Novelty effect may inflate short-term engagement metrics
- Social traffic segment shows no clear Treatment benefit — may warrant separate strategy
- Support ticket volume increase may offset operational costs if not resolved

---

## 10. Screenshots Included

| File | Contents |
|---|---|
| `screenshots/summary_metrics.png` | Control vs Treatment summary table with all 11 metrics |
| `screenshots/hypothesis_test_output.png` | Chi-Square contingency table and t-test bar chart |
| `screenshots/kpi_tree_preview.png` | Full KPI tree diagram preview |

---

## Tools Used

- **Python (pandas, scipy, matplotlib, openpyxl):** Data cleaning, statistical testing, visualisation, Excel generation
- **Excel/openpyxl:** Experiment analysis workbook and summary workbook
- **Matplotlib:** KPI tree diagram and screenshot generation

## Repository Structure

```
part2_kpi_experiment/
├── data/
│   └── campaign_experiment_data.xlsx    ← Raw dataset
├── analysis/
│   ├── experiment_analysis.xlsx         ← Cleaned data + hypothesis test
│   └── hypothesis_test_notes.md         ← Full hypothesis documentation
├── outputs/
│   ├── kpi_tree.png                     ← KPI tree visual
│   ├── experiment_summary.xlsx          ← Summary metrics + segments + guardrails
│   └── recommendation_memo.md           ← Full launch recommendation
├── screenshots/
│   ├── summary_metrics.png
│   ├── hypothesis_test_output.png
│   └── kpi_tree_preview.png
└── README.md
```
