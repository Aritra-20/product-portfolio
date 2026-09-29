# Pharma Sales & Rebate Analytics — ProcDNA Case Study

**Workbook:** [ProcDNA_Case_Study_Rebate_Aritra_Pal.xlsx](ProcDNA_Case_Study_Rebate_Aritra_Pal.xlsx) · Built entirely in Excel (SUMIFS, INDEX/MATCH, VLOOKUP, helper rank columns). Every answer is a live formula back to the raw data.

## The brief

*Charity Pharmaceuticals* (a fictional company in ProcDNA's case) sells one blood-cancer drug, *Philanthropia*, to hospitals and clinics through Group Purchasing Organisations (GPOs). The quarter has closed, and the company needs:

- **Territory-level sales**, weighted by a crediting rule: 340B sales count 0.2×, No Contract sales count 1.5× (except at clinics), everything else counts 1×
- **System-level rebates** under six GPO contracts plus a special group contract (ICOP), each with its own unit tiers
- **Profit** for Q1'25, after manufacturing and shipping costs

The data was about 10,900 invoice lines across three quarters. It had to be stitched to a child→system account hierarchy, 41K ZIP→territory mappings, a 43-member ICOP roster and a rebate criteria sheet.

## Step 1: Clean the data before trusting it

| Issue found | How I handled it |
|---|---|
| 57 rows with a blank GPO | Treated as **No Contract**, which the crediting rule already defines (1.5× hospitals, 1× clinics, no rebate) |
| 5 rows with `#N/A` instead of a quantity | Dropped. There's no way to recover the real number. |
| 10 rows with placeholder Child ID `0` (10 different accounts) | Kept in territory and company totals, excluded from system-level answers. I name-matched all 10 against the mapping: 8 were exact matches, 2 matched a sister site. None were ICOP members, so the rebate answer doesn't change. |
| 262 rows where the 340B flag contradicts Account Type | Ignored the flag and defined 340B by the three 340B GPOs, which is what every rule in the case actually uses |
| One system with both ICOP and non-ICOP children | Split at the child level. ICOP units go to the ICOP contract, and the remaining child earns its own GPO rebate. |
| One invoice ID on two rows (2 and 6 units) | Kept both. It looks like two line items, not a copy-paste duplicate. |

## Step 2: Sales by territory

| | Q3'24 | Q4'24 | Q1'25 |
|---|---|---|---|
| **Nation (weighted)** | 6,290.5 | 8,794.1 | 9,231.1 |

- **Nation sales grew 47% in two quarters.** About 77% of the increase came from the Ally/ION/Onmark/SAN/VS GPO group, and Unity drove most of the rest.
- **Northeast and Southeast lead every quarter**, together about a third of the nation. Account planning should start there.
- **Texas sits in the bottom two every quarter**, which is worth a territory-coverage review.
- **Rebates will follow the growth.** Q2'25 payouts should be budgeted against the growth curve, not a flat quarter.

## Step 3: Top accounts and ICOP

- **Top system by volume (last 2 quarters):** Athena Oncology with 3,896 weighted units, almost double #2, OneOncology (2,144).
- **ICOP in Q1'25:** 9 member systems bought 223 units combined, about 2% of sales. That clears the 50-unit group tier, so **every member earns the top $800/unit rate**, even Mohtaseb Cancer Center with 1 unit. That's what the group contract is designed to do.

## Step 4: Rebates (Q1'25)

Rebates run on **raw units**, not weighted sales, and are calculated at system × GPO level because one system can buy through several GPOs. 340B sales earn nothing.

| GPO | Rebate | Share |
|---|---|---|
| ION | $4.21M | 60% |
| Unity | $1.97M | 28% |
| Onmark | $0.52M | 7% |
| Ally | $0.18M | 3% |
| VS | $0.17M | 2% |
| SAN | $0.03M | <1% |
| **GPO total** | **$7.08M** | |
| ICOP (separate contract) | $0.18M | |
| **All rebates** | **$7.25M** | |

**Top rebate earners:** Athena Oncology ($1.58M), OneOncology ($0.90M), Alliance Cancer Specialists ($0.32M).

## Step 5: Profit, and the real insight

| Q1'25 units | Non-340B units | Revenue (@ $4,500 MRP) | Cost ($2,600 + 7% shipping) | Profit |
|---|---|---|---|---|
| 9,279 | 9,211 | $41.4M | $26.9M | **$14.6M** |

**The insight:** 340B is only 0.7% of volume, so nearly every unit is profitable, at $1,585 a unit (35% of MRP). But **Q1'25 rebates of $7.25M would eat about half of that $14.6M** before it reaches the bottom line. What actually drives profit here is rebate terms, not manufacturing cost. ION alone accounts for 60% of the rebate bill.

---

*Completed as a case discussion exercise for ProcDNA and shared with their permission. Company, product and account data come from the case materials.*
