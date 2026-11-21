# Data Dictionary

## Source columns (`clean_dataset.csv`)
| Field | Type | Meaning | Known issues |
|---|---|---|---|
| order_id | text | Unique order reference (RT-xxxx) | None |
| order_date | date | Date the order was placed | 3 of 11 blank (RT-1002, RT-1006, RT-1011) |
| customer_segment | text | Student, Fresher or Professional | One lower-case value ("student"), fixed in cleaning |
| city | text | Customer city (7 cities) | None |
| category | text | Course Access, Learning Kit or Mentor Session | None |
| quantity | number | Units ordered | RT-1006 is -0.5 (invalid); excluded |
| unit_price | integer (Rs) | List price per unit | None |
| discount_pct | number (%) | Discount applied to the order | None |
| payment_status | text | Paid, Pending, Failed or Refunded | One lower-case value ("paid"), fixed in cleaning |
| order_date_year / _month / _month_name / _quarter | derived | Date parts supplied in the file | Blank when order_date is blank |
| calculated_sales | number (Rs) | quantity x unit_price | Ignores discount and payment status; not used for reporting |

## Derived columns (`clean_orders.csv`)
| Field | Formula |
|---|---|
| valid | quantity > 0 |
| gross | quantity x unit_price |
| discount_amt | gross x discount_pct / 100 |
| net_billed | gross - discount_amt |
| collected | net_billed if payment_status = Paid, else 0 |

## Report metrics
| Metric | Definition |
|---|---|
| Collection rate | collected / net_billed, valid orders only |
| Value at risk | net_billed on Pending + Failed + Refunded orders |
| Discount rate | discount_amt / gross |
| Billed share vs cash share | category's share of net_billed vs its share of collected |
| Annualisation factor | 365 / 16 = 22.8 (sample window 3-18 Jan 2026) |
| ROI | (3-year benefit - 3-year cost) / 3-year cost; undiscounted; year-1 ramp 75% |
