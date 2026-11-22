# Executive Decision Report – Capstone Presentation

> Business Intelligence & Analytics capstone: turning order-level data into an executive-ready decision report with three strategic recommendations, ROI projections and a 90-day implementation plan.

![Python](https://img.shields.io/badge/Python-3.x-blue) ![pandas](https://img.shields.io/badge/pandas-analysis-150458) ![Jupyter](https://img.shields.io/badge/Jupyter-notebook-F37626) 

---

## Overview

This project transforms analytical findings into a 15-slide Business Intelligence report written for C-suite decision makers. It answers one question:

**Demand is there, but is the cash? And which controls protect order value?**

The analysis rebuilds a revenue-to-cash bridge from raw order lines, identifies the key revenue drivers and risk areas, and recommends three operating controls with their business case.

**Audience:** Management Team and Finance Lead
**Data basis:** `clean_dataset.csv`, 11 order records, 3–18 January 2026, 7 cities

---

## Headline Findings

| Metric | Value |
|---|---|
| Gross sales (10 valid orders) | ₹16,285 |
| Net billed (after discounts) | ₹15,019 |
| Collected | ₹8,511 |
| **Collection rate** | **56.7%** |
| Value at risk (pending + failed + refunded) | ₹6,508 (43.3%) |
| Overall discount rate | 7.8% (₹1,266) |

**What the analysis found**

1. **Billing is healthy; collection is not.** Only ₹8,511 of ₹15,019 billed has been paid. ₹2,961 is pending, ₹2,548 was refunded and ₹999 failed.
2. **Course Access bills most and pays least.** It is 47% of billed value but only 21.4% is collected. Learning Kits are 30% of billing and 53% of cash.
3. **Discounts are concentrated and do not buy cash.** Two orders carry 75% of all discount. Discounted orders collect 56.5% vs 56.9% for full-price orders.
4. **Delhi collected nothing; Chennai is the volume base.** Freshers collect least (37.5%) even at 0% discount, which points to payment friction rather than price.

---

## Three Strategic Recommendations

| # | Recommendation | Owner | Key action |
|---|---|---|---|
| 1 | **Recover unpaid orders** | Finance | Automated reminders within 48 hours, retry failed payments, weekly payment-ageing report |
| 2 | **Govern discounts** | Sales head | Cap at 10% without approval, weekly exception report, release fulfilment after payment |
| 3 | **Control refunds and data entry** | Operations / Data | Refund reasons on every case, reject negative quantities and blank dates, fixed pick-lists |

---

## ROI Projections (3-year, undiscounted)

| Scenario | Net benefit | ROI | Payback |
|---|---|---|---|
| **Conservative** (used for approval) | ₹20,264 | 25% | Month 26 |
| Base | ₹127,031 | 155% | Month 11 |
| Stretch | ₹215,224 | 262% | Month 7 |

**Conservative case by recommendation**

| Recommendation | Investment | 3-yr benefit | Net | ROI | Payback |
|---|---|---|---|---|---|
| Rec 1: Recover unpaid orders | ₹56,000 | ₹92,863 | ₹36,863 | 66% | 16 mo |
| Rec 2: Govern discounts | ₹5,000 | ₹9,401 | ₹4,401 | 88% | 21 mo |
| Rec 3: Refunds and data | ₹21,000 | ₹0 | -₹21,000 | -100% | n/a |
| **Total programme** | **₹82,000** | **₹102,264** | **₹20,264** | **25%** | **26 mo** |

> Recommendation 3 is treated as an enabler. It earns nothing in the conservative case and is gated at day 60 unless refund reasons show a preventable cause.

---

## Implementation Timeline

A 90-day plan in three phases with two go/no-go gates.

| Phase | Days | Focus |
|---|---|---|
| 1 | 0–30 | Validation rules live; reminder flow starts on Pending orders |
| 2 | 31–60 | Discount cap enforced; refund reasons captured |
| 3 | 61–90 | Weekly collections report embedded; review and re-baseline |

- **Gate 1 (day 30):** no negative-quantity or blank-date orders accepted; reminder flow live on 100% of Pending orders.
- **Gate 2 (day 60):** Pending share below 10% of billed value (today 19.7%) and every discount above 10% approved.
- **Day 90 target:** collection rate at or above 65% (today 56.7%).

---

## Methodology

A five-stage pipeline with a check at every stage:

1. **Ingest:** load 11 order lines × 14 fields, check types, ranges and missing values.
2. **Clean:** standardise labels (e.g. `student` → `Student`), exclude the invalid order RT-1006 (quantity -0.5), keep undated orders for value but not for timing.
3. **Derive:** `Gross = qty × price`, `Discount = gross × discount %`, `Net billed = gross − discount`, `Collected = net on Paid orders`.
4. **Analyse:** revenue-to-cash bridge, drivers by product / segment / city, discount concentration, risk scoring.
5. **Decide:** conservative / base / stretch lever scenarios, annualised from the 16-day sample, with assumed costs to give ROI and payback.

**Reconciliation check:** Collected ₹8,511 + pending ₹2,961 + refunded ₹2,548 + failed ₹999 = ₹15,019 net billed (difference ₹0).

The source `calculated_sales` column was **not** used because it ignores discounts and totals ₹7,375 above the cash actually collected. The bridge was rebuilt from price, quantity, discount and payment status.

---

## Repository Structure

```
Executive-Decision-Report-Capstone-Presentation/
├── Executive_Decision_Report_Final.pdf   # 15-slide executive report
├── analysis_notebook.ipynb               # Cleaning, metrics, scenarios and ROI model
├── clean_dataset.csv                     # Source order data (11 rows)
├── data_dictionary.md                    # Definitions for every field and metric
└── README.md                             # You are here
```

**Report outline (15 slides):** Title · Executive Summary · Methodology · Data Foundation · Revenue Realisation · Revenue Drivers · Discount Discipline · Customer and Market Value · Risk Areas · Recommendations 1–3 · ROI Projections · Implementation Timeline · Decision Requested

---

## How to Reproduce

1. **Clone the repository**
   ```bash
   git clone https://github.com/prriyayadav/Executive-Decision-Report-Capstone-Presentation.git
   cd Executive-Decision-Report-Capstone-Presentation
   ```
2. **Install dependencies**
   ```bash
   pip install pandas numpy jupyter
   ```
3. **Make sure the dataset is named `clean_dataset.csv`** (the notebook reads this exact filename).
4. **Run the notebook**
   ```bash
   jupyter notebook analysis_notebook.ipynb
   ```
   Run all cells top to bottom. The notebook reproduces every figure in the report and exports `clean_orders.csv`, the validated order-level table with derived columns.

**Notebook sections:** 1 Audit · 2 Clean · 3 Derived metrics · 4 Revenue-to-cash bridge · 5 Drivers by product / segment / city · 6 Discounts · 7 Lever scenarios · 8 ROI model

---

## Limitations

- **Small sample:** only 10 valid orders over 16 days, so patterns are indicative, not proven.
- **No cost or margin data:** programme costs are assumptions, not source data. Replace them with real quotes before release.
- **No refund reason or payment date:** cause and ageing cannot be tested.
- **Annualisation:** annual figures assume the 16-day window is typical of a full year (scaling factor 365/16 ≈ 22.8, with a 75% year-1 ramp).
- **Next step:** re-run the analysis on the full order history after 90 days.

---

## Tools Used

- **Python:** pandas, NumPy
- **Jupyter Notebook:** analysis and ROI modelling
- **Markdown:** documentation and data dictionary
- **PDF report:** executive presentation

---

## Author

**Priya Yadav** · [@prriyayadav](https://github.com/prriyayadav)
