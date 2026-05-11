# Demand Transference Analysis
### How Stockouts Shift Purchasing Power in Licensed Sports Apparel

---

## Overview

When a product goes out of stock, the conventional assumption is that the sale is simply lost. **Demand transference challenges that assumption.** A predictable and quantifiable portion of that lost demand migrates to substitute products within the same assortment — and with the right analytical framework, that behavior can be measured, modeled, and acted on.

This project presents a full demand transference analysis applied to **New York Yankees licensed apparel**, using 18 months of illustrative sales and inventory data across four core SKU families and three retail channels.

> *Note: All data in this project is illustrative and intended to demonstrate the methodology and analytical framework. It is not reflective of actual Yankees or MLB retail results.*

---

## The Business Problem

Licensed sports apparel is one of the most demand-volatile retail categories. Sales spike around Opening Day, playoff runs, player events, and national broadcasts — and stockouts tend to occur precisely at peak demand, the worst possible time.

**Key question this analysis answers:**
> *When a Yankees Home Pinstripe Jersey in size XL goes out of stock, where does that demand actually go — and how much revenue can be recovered?*

---

## Key Findings

| Metric | Value |
|---|---|
| OOS Episodes Analyzed | 847 |
| Average Transfer Rate | 62% |
| Average Lost Demand | 38% |
| Total Recovered Revenue (18 months) | $1,257,155 |
| Total Truly Lost Revenue (18 months) | $647,287 |
| Estimated Recovery Opportunity | $173,000 – $230,000 |

### Strongest Signals
- **Jersey-to-Jersey substitution** captures 20–42% of lost demand — the highest transfer rate observed
- **Replica Jersey Tees** serve as a demand floor, capturing 12–18% of overflow from OOS jerseys
- **In-stadium retail** has the highest transfer rate (71%) vs. e-commerce (58%), reflecting in-person purchase urgency
- **Playoff periods** show the highest transfer rates (79%), making pre-emptive substitute stocking highest-ROI during postseason

---

## Methodology

### Transfer Rate Calculation
```
Transfer Rate (A → B) = ΔSales_B during OOS_A ÷ Lost_Demand_A
```
- **ΔSales_B** — incremental sales uplift of substitute product B during the OOS window of product A
- **Lost_Demand_A** — estimated unfulfilled demand, modeled via SARIMA baseline forecast

### Demand Estimation During Stockout
A **SARIMA** (Seasonal AutoRegressive Integrated Moving Average) model is calibrated against 12 months of pre-event history, with adjustments for:
- Day-of-week and game-day effects
- Promotional calendar (playoffs, All-Star, special events)
- Channel traffic patterns (in-stadium vs. online)
- Comparable non-OOS benchmarks

### Transfer Rate Attribution
Substitute uplift is credited to transference only when the **correlation coefficient** between OOS timing and uplift timing exceeds **0.70**, ensuring signal integrity. Rates are aggregated using a weighted average across all OOS episodes per product pair.

### A Note on Asymmetry
Transfer rates are **intentionally asymmetric** — the rate from Product A → B will not equal B → A. This reflects real consumer behavior: transfer depends on the purchase intent of the customer who encountered the stockout, not simply the product relationship. Symmetric rates in a transfer matrix are a modeling red flag.

---

## Transfer Rate Matrix

| OOS Product | Home Pinstripe Jersey | Away Gray Jersey | Road Alt. Jersey | Replica Jersey Tee |
|---|---|---|---|---|
| **Home Pinstripe Jersey** | — | 38% | 20% | 18% |
| **Away Gray Jersey** | 42% | — | 22% | 14% |
| **Road Alt. Jersey** | 35% | 28% | — | 12% |
| **Replica Jersey Tee** | 22% | 19% | 8% | — |

*Darker = higher substitution rate. Values represent 18-month weighted averages.*

---

## Seasonal Transfer Rate Variation

| Period | Avg. Transfer Rate | Lost Demand | Inventory Risk |
|---|---|---|---|
| Opening Day +2 Weeks (Mar–Apr) | 74% | 26% | 🔴 Critical |
| Regular Season (Apr–Sep) | 63% | 37% | 🟠 High |
| Playoffs (Oct) | 79% | 21% | 🔴 Critical |
| Off-Season (Nov–Feb) | 48% | 52% | 🟡 Moderate |
| Special Events | 41% | 59% | 🔴 Extreme |

---

## Repository Contents

```
demand-transference/
│
├── README.md                                      ← You are here
│
├── whitepaper/
│   └── DT_WhitePaper.pdf                          ← Full white paper
│
├── sql/
│   └── yankees_demand_transference.sql            ← PostgreSQL DDL + sample data + 8 queries (in progress)
│
├── dashboard/
│   └── Yankees_Demand_Transference_Dashboard.xlsx ← Excel dashboard (in progress)
│
└── images/
    ├── sankey_demand_flow.png                     ← Demand flow diagram
    └── transfer_rate_matrix.png                   ← Heat map matrix
```

---

## SQL Data Model

The PostgreSQL schema includes 7 tables:

| Table | Description |
|---|---|
| `products` | SKU catalog with family, size, and pricing |
| `channels` | Retail channels (stadium, store, e-commerce) |
| `seasons` | MLB calendar periods with risk classifications |
| `inventory_snapshots` | Daily on-hand inventory with auto-computed OOS flag |
| `sales_transactions` | Daily sales by SKU and channel |
| `oos_events` | Discrete stockout episodes with estimated lost demand |
| `transfer_rates` | Aggregated substitution rates with affinity scores |

### Analytical Queries Included
1. Transfer Rate Matrix — cross-SKU family substitution rates
2. OOS Episode Summary — duration, lost demand, lost revenue
3. Revenue Recovered vs. Truly Lost — by SKU family
4. Substitute Uplift Detection — compares sales during vs. pre-OOS
5. Seasonal Transfer Rate Analysis — by period and channel
6. Top Substitute Pairs — ranked by affinity score
7. Channel Revenue Recovery — transfer rates by retail channel
8. Asymmetry Check — flags any symmetric rate pairs as modeling errors

---

## Excel Dashboard

Five-tab workbook aligned to the white paper:

| Tab | Contents |
|---|---|
| **Dashboard** | Master view — KPI tiles, transfer matrix, financial summary, seasonal & channel breakdowns |
| **Transfer Matrix** | Full 12-row rate table with affinity scores, substitute tiers, asymmetry check |
| **OOS Events** | 12 sample OOS episodes with dates, duration, lost demand and revenue |
| **Financial Summary** | Revenue recovered vs. lost by SKU family + recovery opportunity analysis |
| **SQL Query Reference** | Quick-reference card mapping analytical questions to queries |

---

## Recommendations

1. **Implement a Transfer Rate Dashboard** — Track rates by SKU pair, channel, and season; alert when high-velocity SKUs drop below 21-day cover
2. **Define Substitute Pairs for All High-Velocity SKUs** — Designate top-2 substitutes per SKU to drive automatic safety-stock uplift rules
3. **Run Seasonal Inventory Reviews Tied to the MLB Calendar** — 60-day pre-season reviews with worst-case OOS scenario modeling before peak periods

---

## Author

Personal portfolio project — illustrative analysis demonstrating demand transference methodology applied to licensed sports apparel retail.

---

*This project is for portfolio and educational purposes. Data is illustrative and not reflective of actual New York Yankees or MLB retail operations.*
