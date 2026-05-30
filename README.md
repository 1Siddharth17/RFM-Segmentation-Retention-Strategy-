# &#x20;D2C Churn Intelligence — Part 2

## RFM Segmentation \& Retention Strategy


## &#x20;Project Overview

This is **Part 2** of a 3-part D2C churn intelligence system. After understanding churn drivers (Part 1), we now segment customers by RFM + behavioral signals and develop **segment-specific retention strategies**.

**Business Goal:**

> Replace blanket discounting with \*\*targeted, data-driven retention campaigns\*\* that maximize ROI per retention budget rupee.

**Expected Business Impact:**

* 15–25% improvement in overall churn rate (90 days post-launch)
* 3–8x ROI on retention spending (vs 0.5–1.5x for generic discounting)
* ₹300K–₹400K net retention value annually


## &#x20;Quick Start

### Option 1: Run in Google Colab (Recommended)

1. Open [`rfm\_segmentation.ipynb`](rfm_segmentation.ipynb) in Google Colab
2. Click **"Run all"** (executes in \~5 minutes)
3. Review charts and segment profiles
4. Download `segments.csv` from outputs folder

### Option 2: Run Locally (Python 3.8+)

```bash
# Install dependencies
pip install -r requirements.txt

# Run Jupyter notebook
jupyter notebook rfm\_segmentation.ipynb
```

### Option 3: Quick Reference (No Code)

* Read [`retention\_strategy.md`](retention_strategy.md) for segment details
* Review [`manual\_review\_cases.md`](manual_review_cases.md) for edge cases
* Check outputs folder for generated CSV files


## &#x20;Deliverables

### 1\. **rfm\_segmentation.ipynb** — Main Notebook (28 cells)

**What:** Google Colab-compatible Jupyter notebook  
**Includes:**

* Synthetic data generation (self-contained; no uploads needed)
* RFM feature engineering (6+ advanced features beyond base R/F/M)
* Behavioral signal calculation (support, engagement, intervention history)
* Customer segmentation logic (rule-based, interpretable, business-explainable)
* 5+ high-quality visualizations
* Segment analysis \& export

**Outputs:**

* `segments.csv` (5,000+ customers with segment assignments)
* `segment\_profiles.csv` (aggregate characteristics by segment)
* `segment\_summary.csv` (business metrics: churn, revenue, engagement)
* 6+ PNG charts (exported to `outputs/charts/`)

### 2\. **retention\_strategy.md** — Detailed Playbook (7 segments)

**What:** Executive-level retention action plan  
**For Each Segment:**

* Profile \& business context
* Why they matter (revenue contribution, churn risk)
* 5+ specific retention actions (with mechanics, timeline, expected ROI)
* Budget \& cost-benefit analysis
* Success metrics (what to measure monthly)

**Segments Covered:**

1. &#x20;**Champions** — VIP programs, loyalty, events
2. &#x20;**Loyal Customers** — Rewards, recommendations, referrals
3. &#x20;**Potential High-Value** — Cross-sell, education, onboarding
4. &#x20;**At-Risk** — Proactive outreach, targeted incentives
5. &#x20;**Dormant** — Win-back campaigns, heavy incentives
6. &#x20;**Discount-Sensitive** — Loyalty conversion, flash sales, bundles
7. **High-Support Needs** — Dedicated CS, resolution guarantees, consultations

### 3\. **manual\_review\_cases.md** — Edge Cases (10 Profiles)

**What:** Real-world ambiguous cases requiring human judgment  
**Includes:**

* Customer ID + key features
* Why segmentation is ambiguous
* Root cause analysis
* Specific retention recommendation
* Expected business outcome

**Examples:**

* High spender with many complaints (High-Support Needs)
* Loyal customer who suddenly went dormant (Seasonal buyer; different strategy)
* New customer with exceptional engagement (VIP pathway)
* Discount-only buyer with low engagement (Deprioritize retention)

### 4\. **segments.csv** — Customer Segment Assignments

**Columns:**

* `customer\_id`
* `segment\_name` (Champions, Loyal, At-Risk, Dormant, etc.)
* `recency\_days`, `frequency`, `monetary\_value`
* `support\_ticket\_count`, `return\_rate`, `category\_diversity`
* `discount\_usage\_rate`, `engagement\_score`, `churn\_label`
* `tenure\_days`, `plan\_type`

**Use For:**

* Email platform segmentation (send segment-specific campaigns)
* CRM systems (tag customers by segment)
* Dashboard tracking (monthly segment refresh)
* ML modeling (Part 3 will use segment features)

### 5\. **requirements.txt** — Python Dependencies

All packages needed to run notebook locally or in Colab.


## &#x20;Key Findings Summary

### Segment Sizes \& Churn Risk

|Segment|Size|Churn Rate|Annual Revenue|Priority|
|-|-|-|-|-|
|**Champions**|250|5%|15–20%|Maintenance|
|**Loyal Customers**|900|10–15%|20–25%|Medium|
|**Potential High-Value**|350|15–20%|12–15%|Medium|
|**At-Risk**|900|35–50%|8–10%| HIGH|
|**Dormant**|1100|60–75%|<5%| HIGH|
|**Discount-Sensitive**|700|25–35%|5–8%|Medium|
|**High-Support Needs**|500|40–55%|5–8%| HIGH|

### Budget Allocation (₹5M Annual)

|Tier|Segments|Budget|Expected ROI|Time to Implement|
|-|-|-|-|-|
| **Tier 1**|At-Risk, Dormant, High-Support|₹2.5M (50%)|7.4x|2–4 weeks|
| **Tier 2**|Discount-Sensitive, Potential High-Value|₹2.0M (40%)|6.0x|4–6 weeks|
| **Tier 3**|Champions, Loyal (maintenance only)|₹0.5M (10%)|54.5x|Ongoing|


## &#x20;Segmentation Methodology

### Step 1: RFM Feature Engineering

**Recency, Frequency, Monetary** scored separately using percentile ranks:

* **Recency:** Lower is better (0–180+ days since last purchase)
* **Frequency:** Higher is better (1–20 orders)
* **Monetary:** Higher is better (₹0–₹10,000+)

**Advanced Features** (for richer segmentation):

* `avg\_order\_value` — Average spending per transaction
* `category\_diversity` — # unique product categories purchased
* `discount\_usage\_rate` — % of orders using discounts
* `return\_rate` — % of orders that are returns
* `repeat\_purchase\_ratio` — Frequency relative to max possible

### Step 2: Behavioral Signals

**Non-RFM features** that predict churn:

* `support\_ticket\_count` — # support interactions (more = friction)
* `unresolved\_tickets` — Unresolved/escalated complaints (red flag)
* `intervention\_count` — Prior retention campaign contacts
* `complaint\_severity` — Ratio of unresolved to total tickets (0–1)
* `engagement\_score` — Composite metric (0–100)

### Step 3: Segmentation Logic

**Rule-based assignment** (interpretable, business-explainable):

```
IF (R+F+M score ≥12 AND support\_tickets ≤1 AND complaint\_severity <0.3):
    IF monetary > ₹2000:
        Assign "Champions"
    ELSE:
        Assign "Loyal Customers"

ELIF (R ≥ 4 AND F ≥ 3 AND support\_tickets ≤ 2):
    Assign "Potential High-Value"

ELIF (support\_tickets ≥ 3 OR complaint\_severity > 0.5):
    Assign "High-Support Needs"

ELIF (R ≤ 2 AND tenure > 60 days):
    Assign "Dormant"

ELIF (discount\_usage\_rate > 0.5 AND monetary < ₹1000):
    Assign "Discount-Sensitive"

ELSE:
    Assign "At-Risk"
```

**Why This Logic?**

* **Champions** = Best RFM + low friction → preserve with VIP treatment
* **Loyal** = Good RFM + responsive → nurture toward Champions
* **High-Value** = High spend but low frequency → cross-sell to habit-form
* **High-Support Needs** = Many tickets → fix friction before losing them
* **Dormant** = Silent 6+ months → heavy reactivation needed
* **Discount-Sensitive** = Deal-driven, low spend → convert to loyalty
* **At-Risk** = Everything else → proactive outreach before churn
  

## &#x20;Business Reasoning for Each Segment

### Champions → VIP Loyalty

**Rationale:** 1 Champion referral worth 3 new customers. Retention is 90x cheaper than replacement.  
**Investment:** ₹500/year (events, consultations, exclusivity)  
**ROI:** 90–120x

### Loyal → Engagement + Referrals

**Rationale:** Reliable repeat buyers; graduate 15–20% to Champions within 12 months.  
**Investment:** ₹200/year (rewards, recommendations, referral incentives)  
**ROI:** 8–10x

### Potential High-Value → Cross-Sell \& Education

**Rationale:** 1 additional purchase/year = ₹500K segment revenue. Low-hanging fruit.  
**Investment:** ₹150/year (content, bundles, personalization)  
**ROI:** 7.5–10x

### At-Risk → Proactive Outreach

**Rationale:** Last opportunity to re-engage before full dormancy. Win-back < acquisition cost.  
**Investment:** ₹250/year (email sequences, targeted discounts)  
**ROI:** 9.6–12x

### Dormant → Heavy Reactivation

**Rationale:** Have 35–40% conversion rates on big incentives. Last-ditch effort.  
**Investment:** ₹300/year (35–40% discounts, samples, SMS)  
**ROI:** 10–12x

### Discount-Sensitive → Loyalty Conversion

**Rationale:** Convert from deal-hunters to loyalty-driven. Preserve volume while improving margin.  
**Investment:** ₹180/year (flash sales, bundles, loyalty program)  
**ROI:** 3.1–4.3x (lower, but necessary to prevent defection to competitors)

### High-Support Needs → Friction Elimination

**Rationale:** Churn is due to **unresolved issues**, not low loyalty. Fix the root cause.  
**Investment:** ₹400/year (dedicated CS agent, consultation, gestures)  
**ROI:** 2.2–3.2x (lowest ROI, but essential for brand reputation)


## &#x20;Implementation Roadmap

### Phase 1: Foundation (Week 1–2)

* \[ ] Run notebook; generate segments.csv
* \[ ] Share segment profiles with leadership (get buy-in)
* \[ ] Set up email/SMS platform segmentation
* \[ ] Design email templates for each segment
* \[ ] Hire 2–3 dedicated support agents (for High-Support Needs)

### Phase 2: Tier 1 Launch (Week 3–4)

* \[ ] Launch At-Risk proactive outreach campaign
* \[ ] Launch Dormant win-back campaigns (35–40% offer)
* \[ ] Activate dedicated support agent program
* \[ ] Daily tracking of opens, clicks, conversions, ROI

### Phase 3: Tier 2 \& 3 Launch (Week 5–8)

* \[ ] Roll out Loyalty programs (Loyal, Discount-Sensitive, Champions)
* \[ ] Launch cross-sell bundles (Potential High-Value)
* \[ ] VIP events + consultation scheduling (Champions)
* \[ ] A/B test email subject lines, offers, timing

### Phase 4: Optimization \& Scale (Week 9+)

* \[ ] Monthly segment refresh (RFM recalculation)
* \[ ] Adjust budgets by segment ROI (move spend to winners)
* \[ ] Refine audience targeting based on response data
* \[ ] Prepare for Part 3: Predictive churn modeling


## &#x20;Success Metrics (Track Monthly)

### Overall Metrics

* **Churn Rate:** Current 43% → Target 28% (6-month goal)
* **Retention Spend ROI:** Current N/A → Target 5–7x average across segments
* **Customer Lifetime Value:** Current ₹1,800 → Target ₹2,200+ (22% lift)

### Segment-Specific Metrics

|Segment|Target Email Open|Target Conversion|Target Retention Rate|
|-|-|-|-|
|**Champions**|40%+|N/A (loyalty)|95%+|
|**Loyal**|25–35%|8–12%|85%+|
|**Potential High-Value**|20–30%|12–18%|80%+|
|**At-Risk**|18–28%|12–18%|65%–70%|
|**Dormant**|15–25%|8–15%|45%–50%|
|**Discount-Sensitive**|16–26%|10–15%|75%+|
|**High-Support Needs**|20–30%|10–15% (support)|60%–65%|


## &#x20;Integration with Other Systems

### Email Platform (Klaviyo, HubSpot, etc.)

* Import `segments.csv` customer\_id + segment\_name
* Create segment lists per cohort
* Map segment to email campaigns (automation flows)

### CRM (Salesforce, Pipedrive, etc.)

* Add `segment\_name` and `engagement\_score` fields to customer records
* Tag customers by segment for sales/CS context
* Use engagement\_score to prioritize support tickets

### Analytics Dashboard (Tableau, Looker, etc.)

* Pull segments.csv monthly
* Track churn rate, revenue, campaign performance by segment
* Visualize segment transitions (who's moving between segments?)

### Part 3 (Predictive Modeling — Future)

* Use segment features as inputs to churn prediction model
* Segments inform model feature engineering
* Calibration: Validate model predictions against actual segment churn rates


## &#x20;Documentation Index

|Document|Audience|Purpose|
|-|-|-|
|[`rfm\_segmentation.ipynb`](rfm_segmentation.ipynb)|Data Scientists, Analysts|Runnable notebook; segment generation|
|[`retention\_strategy.md`](retention_strategy.md)|Marketing, Retention, Leadership|Detailed action plans per segment|
|[`manual\_review\_cases.md`](manual_review_cases.md)|Data Scientists, CRM Strategists|Edge case handling, business reasoning|
|[`segments.csv`](segments.csv)|Email Platforms, CRM, Analytics|Customer segment assignments (5,000+ rows)|
|[`requirements.txt`](requirements.txt)|Developers|Python package dependencies|
|**README\_PART2.md** (this file)|Everyone|Project overview, quick start, methodology|


## &#x20;Critical Assumptions \& Caveats

1. **RFM isn't perfect.** It's a useful heuristic, not a causal model. Combine with behavioral signals (support, engagement) for better segmentation.
2. **Segment boundaries are fuzzy.** Some customers fit multiple segments (see `manual\_review\_cases.md`). Use business judgment for edge cases.
3. **Segments need monthly refresh.** RFM changes as customers purchase. Rebuild segments on 1st of month to catch transitions (At-Risk → Dormant, Dormant → Loyal, etc.).
4. **Intervention costs are estimates.** Actual costs depend on your:

   * Email/SMS platform pricing
   * Customer support headcount \& salary
   * Discount depth (30% vs 40% discount has different margins)
   * Adjust ROI estimates to your specific unit economics
5. **No data leakage in segmentation.** All features are as-of snapshot date (2024-06-30). Future purchases are excluded to prevent temporal bias.


## &#x20;Common Pitfalls (Avoid These)

### &#x20;Don't:

* **Discount everyone equally.** Discount-Sensitive needs deals; Champions need exclusivity.
* **Ignore support quality.** High-Support customers aren't disloyal; they're frustrated. Fix issues, not discounts.
* **Set it and forget it.** Segments change monthly. Refresh segmentation every 30 days.
* **Measure only open rates.** Track conversion, retention rate, and ROI—not just engagement vanity metrics.
* **Treat new segment assignments as permanent.** Monitor segment transitions; move customers as behavior changes.

### &#x20;Do:

* **Personalize by segment.** A email to Champions should feel exclusive; At-Risk should feel urgent.
* **Test \& optimize.** A/B test subject lines, offer amounts, timing. Scale what works.
* **Track monthly cohorts.** Measure retention separately for customers assigned to each segment when they were targeted.
* **Document business decisions.** When deviating from logic (e.g., spending ₹500 on a ₹900-spend customer), explain why in CRM notes.


## 

