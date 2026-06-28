# Recommendation Memo
## Campaign Experiment Analysis – Onboarding & Activation Campaign

**To:** Leadership Team  
**From:** Business Analytics Team  
**Date:** June 2026  
**Subject:** Launch Recommendation – New Onboarding Campaign (A/B Test Results)

---

## 1. Executive Summary

The new onboarding and activation campaign produced **statistically significant improvements**
in paid conversion rate and user engagement. Treatment users converted at **6.99%** versus
**3.17%** in the control group — a **+3.82 percentage point lift** (approximately +120% relative
improvement), with a p-value of **0.0017**, well below the 0.05 significance threshold.

**Recommendation: Launch to all users, with phased monitoring of guardrail metrics.**

The data strongly supports launch. However, three guardrail concerns (elevated support ticket rate,
lower average revenue per converted user, and a small refund signal) should be actively monitored
for the first 30 days post-launch.

---

## 2. North Star Metric

**Paid Conversion Rate** is the single most important metric for this experiment.

**Why this metric?**
- It directly measures the campaign's ultimate business goal: turning free/trial users into paying customers.
- It connects linearly to subscription revenue growth.
- All other funnel metrics (landing page visits, trial starts, onboarding completion) are only valuable
  insofar as they contribute to paid conversion.

**Why not other metrics?**
- *Engagement Score* is a leading indicator but not monetised directly.
- *Onboarding Completion Rate* is a driver, not an outcome.
- *Revenue per User* is important but depends heavily on plan mix, which the experiment does not directly control.

**Risk of blind optimisation:**
If the campaign is optimised purely for conversion rate without quality checks, the company may
acquire users who churn quickly, refund, or demand excessive support — reducing lifetime value.
This is why guardrail metrics are essential.

---

## 3. KPI Tree Explanation

The North Star Metric (Paid Conversion Rate) is driven by three primary pillars:

```
PAID CONVERSION RATE (North Star)
├── DRIVER 1: Acquisition & Top-of-Funnel
│   ├── Landing Page Visit Rate (C: 63.6% → T: 72.6%)
│   ├── Trial Start Rate (C: 25.1% → T: 29.1%)
│   ├── Traffic Source Mix (Referral shows highest lift for Treatment)
│   └── Device & Region Segment Reach
│
├── DRIVER 2: Activation & Onboarding Quality
│   ├── Onboarding Completion Rate (C: 15.6% → T: 21.3%)
│   ├── Days to Convert (C: 8.9 days → T: 6.4 days)
│   ├── UX / Campaign Flow Quality
│   └── Support Ticket Rate (⚠ elevated in Treatment)
│
└── DRIVER 3: Engagement & Retention Value
    ├── Engagement Score (C: 57.0 → T: 62.9) *** p < 0.0001
    ├── Avg Revenue / Converted User (⚠ lower in Treatment)
    ├── Plan Upgrade Rate
    └── Refund Rate (⚠ small signal in Treatment)
```

**Guardrail Metrics:** Refund Rate, Support Ticket Rate, Avg Revenue per Converted User,
Segment-level Conversion Decline

See `outputs/kpi_tree.png` for the full visual diagram.

---

## 4. Experiment Result Summary

| Metric | Control | Treatment | Change | Status |
|---|---|---|---|---|
| Sample Size | 693 | 715 | +22 | Balanced |
| Landing Page Visit Rate | 63.64% | 72.59% | +8.95 pp | ✅ Positive |
| Trial Start Rate | 25.11% | 29.09% | +3.98 pp | ✅ Positive |
| Onboarding Completion Rate | 15.58% | 21.26% | +5.68 pp | ✅ Positive |
| **Paid Conversion Rate** | **3.17%** | **6.99%** | **+3.82 pp** | **✅ Significant** |
| Avg Revenue per User | $51.75 | $53.88 | +$2.13 | ✅ Positive |
| Avg Revenue per Converted User | $1,630 | $770 | -$860 | ⚠ Monitor |
| Refund Rate | 0.00% | 0.42% | +0.42 pp | ⚠ Monitor |
| Support Ticket Rate | 14.72% | 24.76% | +10.04 pp | ⚠ Elevated |
| Avg Engagement Score | 57.03 | 62.93 | +5.90 pts | ✅ Significant |
| Avg Days to Convert | 8.86 days | 6.40 days | -2.46 days | ✅ Faster |

The funnel improvements are consistent across all stages — from top-of-funnel (landing page visits)
through mid-funnel (trial starts, onboarding) to bottom-of-funnel (paid conversion).

---

## 5. Hypothesis Test Interpretation

**Test 1 – Paid Conversion Rate (Chi-Square Test)**
- H₀: Conversion rates are equal in both groups.
- H₁: Conversion rates differ between groups.
- Result: Chi-Square = 9.80, p = 0.0017 → **Reject H₀**
- Interpretation: The treatment has a statistically significant positive effect on paid conversion.

**Test 2 – Engagement Score (Independent t-test)**
- H₀: Mean engagement scores are equal.
- H₁: Mean engagement scores differ.
- Result: t = 7.94, p < 0.0001 → **Reject H₀**
- Interpretation: Treatment users are significantly more engaged, supporting the conversion findings.

Both tests confirm the campaign's effectiveness with high statistical confidence.

---

## 6. Guardrail Analysis

| Guardrail | Control | Treatment | Risk | Assessment |
|---|---|---|---|---|
| Refund Rate | 0.00% | 0.42% | LOW | Small absolute value; 3 refunds out of 715 treatment users. Acceptable but requires a 0% target trajectory. |
| Support Ticket Rate | 14.72% | 24.76% | MEDIUM | +10 pp elevation suggests users may be confused or encountering friction in the new campaign flow. UX audit is strongly recommended before full launch. |
| Avg Rev / Converted User | $1,630 | $770 | MEDIUM | Treatment converts more users but at lower per-user revenue. Likely driven by more Free→Paid conversions (lower plan tier). This is not necessarily bad — more volume at lower price can exceed total revenue from fewer high-value converts. Needs LTV modelling. |
| Days to Convert | 8.9 days | 6.4 days | LOW (positive) | Faster conversion is a positive guardrail outcome. |
| Engagement Score | 57.0 | 62.9 | LOW (positive) | Higher engagement reduces churn risk. |

The most significant guardrail concern is the **elevated support ticket rate** in Treatment.
This warrants a UX review of the new onboarding flow to identify specific pain points.

---

## 7. Segment-Level Insights

**By Region:** Treatment outperforms Control in all four regions. North shows the highest lift
(8.89% vs 3.45%), making it a strong candidate for priority rollout.

**By Device:** All device types show Treatment lift. Mobile and Tablet show the strongest
relative improvements.

**By Traffic Source:** Referral traffic shows the highest Treatment conversion rate (10.99%).
Paid Search and Organic also improved significantly. Social media traffic is the only segment
where Control performed comparably to Treatment (7.69% vs 6.02%) — Social users may respond
differently to the new experience.

**By Plan Type:** Free plan users in Treatment show the strongest improvement (9.24% vs 3.05%),
suggesting the campaign is highly effective at converting free-tier users to paid. This also
explains the lower average revenue per converted user (Free→Paid converts at a lower price point).

---

## 8. Final Recommendation

### **RECOMMENDATION: LAUNCH to all users**
*(with phased guardrail monitoring)*

**Rationale:**
1. The conversion rate improvement (+120% relative lift) is statistically significant (p = 0.0017).
2. Engagement scores are significantly higher (p < 0.0001), indicating genuine user value creation.
3. Faster time-to-convert (6.4 vs 8.9 days) reduces acquisition cost and improves cash flow timing.
4. All regions and device segments benefit from the treatment.
5. Overall revenue per user improved (+$2.13 per user).

**Conditions for launch:**
- UX team to audit the onboarding flow to understand and reduce elevated support ticket rate.
- Customer success team to proactively reach out to new Treatment converters in first 30 days.
- Revenue quality monitoring: track LTV of Treatment cohort for 60 and 90 days.
- Set refund rate alert threshold at 1%; trigger review if breached.

---

## 9. Risks and Limitations

1. **Short observation window:** Revenue and retention data is limited to 30 days. Longer-term
   churn and LTV are unknown. Treatment's lower avg revenue per convert could compound over time
   if lower-tier converts churn faster.

2. **Support ticket volume:** If the elevated support ticket rate persists, it increases operational
   costs that may offset conversion gains.

3. **Social traffic segment:** Treatment underperforms control for Social traffic source.
   Consider retaining the control experience for Social-acquired users while testing further.

4. **Sample size for some segments:** Smaller sub-segments (e.g., Tablet users, Referral traffic)
   have lower sample sizes, which reduces statistical confidence of those specific findings.

5. **Novelty effect:** Some of the engagement and conversion lift may reflect novelty bias —
   users engaging more because the experience is new. A longer experiment window or holdout
   analysis would validate persistence of effects.

---

## 10. Next Steps

| Priority | Action | Owner | Timeline |
|---|---|---|---|
| HIGH | UX audit of new campaign onboarding flow | Product / UX | Week 1 |
| HIGH | Set up 30-day guardrail monitoring dashboard | Analytics | Pre-launch |
| HIGH | Launch campaign to all users | Engineering | Week 2 |
| MEDIUM | Analyse 60-day LTV of Treatment cohort | Analytics | 8 weeks post-launch |
| MEDIUM | Run targeted test for Social traffic segment | Growth | Month 2 |
| LOW | Explore upsell paths to convert Free-tier users to higher plan tiers | Product | Quarter 3 |
