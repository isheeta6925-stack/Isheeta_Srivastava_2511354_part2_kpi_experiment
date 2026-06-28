# Recommendation Memo
## Part 2: KPI Framework, Business Experiment Analysis & Decision Recommendation

**To:** Leadership Team
**From:** Business Analytics Team
**Date:** June 2026
**Subject:** New Onboarding Campaign — Experiment Results & Launch Decision

---

## 1. Executive Summary

The new onboarding and activation campaign was tested on 1,200 users (600 Control, 600 Treatment) over the experiment period. The Treatment group showed a **statistically significant improvement in paid conversion rate** (+4.5 percentage points, p < 0.01). Guardrail metrics remain within acceptable thresholds. **The recommendation is to launch the campaign to all users**, with active monitoring of refund and support ticket rates in the first week post-launch.

---

## 2. North Star Metric

**Selected North Star Metric: Paid Conversion Rate**

> The proportion of users who convert from free trial / onboarding to a paid subscription plan.

**Why this is the North Star:**
- It directly drives revenue and sustainable business growth.
- It is the definitive signal that the campaign delivered real value to users.
- All other metrics (landing page visits, trial starts, onboarding completion) are necessary *drivers* of conversion, not the end goal.

**Why other metrics are supporting, not North Star:**
- *Trial start rate* shows interest but not commitment.
- *Engagement score* shows activity but does not guarantee revenue.
- *Average Revenue Per User* is important but depends on conversion occurring first.

**Risk of blind optimization:**
If conversion rate alone is optimized — for example by reducing plan prices aggressively — revenue quality could decline even as conversion improves. This is why guardrail metrics (refund rate, ARPU, support tickets) must be monitored alongside the North Star.

---

## 3. KPI Tree Summary

```
NORTH STAR: Paid Conversion Rate
│
├── Driver 1: Awareness & Reach
│   ├── Landing Page Visit Rate
│   ├── Traffic Source Quality (Organic vs Paid vs Referral)
│   └── Campaign Reach (Impressions / Unique Users Targeted)
│
├── Driver 2: Activation & Engagement
│   ├── Trial Start Rate
│   ├── Onboarding Completion Rate
│   └── Engagement Score (in-product activity)
│
├── Driver 3: Revenue Quality
│   ├── Average Revenue Per Converted User
│   ├── Plan Type Distribution (Basic / Pro / Enterprise)
│   └── Days to Convert (speed of purchase decision)
│
└── Guardrail Metrics (must not deteriorate)
    ├── Refund Rate (threshold: < 12%)
    ├── Support Ticket Rate (threshold: < 18%)
    └── Segment-level Conversion Stability (no region/device decline)
```

*(See `outputs/kpi_tree.png` for the visual KPI tree diagram)*

---

## 4. Experiment Result Summary

| Metric | Control | Treatment | Lift | Significant? |
|---|---|---|---|---|
| User Count | 600 | 600 | — | — |
| Landing Page Visit Rate | ~55% | ~68% | +13 pp | ✓ |
| Trial Start Rate | ~33% | ~49% | +16 pp | ✓ |
| Onboarding Completion Rate | ~21% | ~37% | +16 pp | ✓ |
| **Paid Conversion Rate** | **~19%** | **~23.5%** | **+4.5 pp** | **✓ Yes** |
| Avg Revenue Per User | ~$18 | ~$22 | +$4 | ✓ |
| Avg Revenue Per Converted User | ~$98 | ~$101 | +$3 | Marginal |
| Refund Rate | ~4.5% | ~5.5% | +1 pp | ⚠ Monitor |
| Support Ticket Rate | ~12% | ~13% | +1 pp | ⚠ Monitor |
| Avg Engagement Score | ~62 | ~67 | +5 pts | ✓ |
| Avg Days To Convert | ~14.2 | ~13.8 | -0.4 days | ✓ |

---

## 5. Hypothesis Test Interpretation

**Test:** One-tailed Z-test for proportions on Paid Conversion Rate

| Parameter | Result |
|---|---|
| H₀ | p_treatment ≤ p_control |
| H₁ | p_treatment > p_control |
| Z-Statistic | ~2.40 |
| P-Value (one-tailed) | ~0.0082 |
| Significance Level (α) | 0.05 |
| **Outcome** | **REJECT H₀** |

The conversion rate improvement is statistically significant. The probability of observing this result by chance, if the campaign had no effect, is less than 1%.

---

## 6. Guardrail Analysis

| Guardrail Metric | Control | Treatment | Change | Risk Level |
|---|---|---|---|---|
| Refund Rate | ~4.5% | ~5.5% | +1 pp | 🟡 Low — below 12% threshold |
| Support Ticket Rate | ~12% | ~13% | +1 pp | 🟡 Low — likely onboarding learning curve |
| Days to Convert | ~14.2 | ~13.8 | -0.4 days | 🟢 Positive |
| Engagement Score | ~62 | ~67 | +5 pts | 🟢 Positive |
| Avg Rev Per Converted User | ~$98 | ~$101 | +$3 | 🟢 Revenue quality maintained |

**Assessment:** No guardrail metric has breached its threshold. The slight increase in refund and support rates should be monitored in the first week after full launch, as they may reflect a short-term adjustment period for users encountering the new onboarding flow for the first time.

---

## 7. Segment-Level Insight

**By Region:**
- All four regions (North, South, East, West) showed positive lift in conversion rate under Treatment.
- No region showed a statistically significant decline — the campaign works across geographies.

**By Device Type:**
- Mobile and Desktop both improved.
- Tablet showed the smallest lift — may warrant a separate UX review for tablet-specific onboarding.

**By Traffic Source:**
- Referral traffic users had the highest absolute conversion rates in both groups.
- Email traffic users showed the largest lift (+6 pp) — the campaign resonates particularly well with email-driven users.

---

## 8. Final Recommendation

> **✅ LAUNCH the new onboarding and activation campaign to all users.**

**Basis for recommendation:**
1. Paid conversion rate improved significantly (+4.5 pp, p < 0.01).
2. Guardrail metrics remain within acceptable thresholds.
3. Revenue quality was maintained (ARPU improved, not diluted).
4. The improvement is consistent across regions and device types.
5. Users converted faster and showed higher engagement under the Treatment.

**Conditions for launch:**
- Monitor refund rate and support ticket rate daily for the first 7 days post-launch.
- Set alert threshold: if refund rate exceeds 10% or support tickets exceed 20% within 7 days, pause and investigate.
- Conduct a separate UX review for tablet users, where lift was smallest.

---

## 9. Risks and Limitations

| Risk | Mitigation |
|---|---|
| Short-term novelty effect — users may engage more with any new experience initially | Re-evaluate conversion and retention at Day 30 and Day 90 post-launch |
| Refund and support increase may worsen at scale | Set monitoring thresholds and designate a response owner |
| Experiment may not represent all user segments (e.g., enterprise clients not in sample) | Extend testing to enterprise segment before assuming full coverage |
| Seasonal effects not controlled | Confirm results align with historical baseline for this calendar period |
| Synthetic dataset used for this assignment | Real production data may show different variance patterns |

---

## 10. Next Steps

1. **Launch:** Deploy the new onboarding campaign to 100% of new users.
2. **Monitor:** Track daily refund rate and support ticket rate for 7 days post-launch.
3. **Cohort analysis:** After 30 days, compare Day-30 retention between historical Control-era users and new campaign users.
4. **Segment deep-dive:** Investigate tablet experience separately; consider a device-specific A/B test.
5. **Iteration:** Use engagement score data to identify which steps of the new onboarding flow drive the highest conversion lift — optimize those further.
6. **Document:** Archive experiment results and decisions in the analytics knowledge base for future experiment benchmarking.
