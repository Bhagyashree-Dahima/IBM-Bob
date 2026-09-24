# E-Commerce Website Log Analysis

A self-contained Python data-analysis project that loads, cleans, and analyses
**172,838 e-commerce web-server sessions** from `E-commerce Website Logs new.csv`
and answers 14 targeted business questions with supporting charts.

---

## Project Structure

```
ibm bob/
├── E-commerce Website Logs new.csv   ← raw dataset (required)
├── ecommerce_analysis.py             ← main analysis script
├── requirements.txt                  ← Python dependencies
├── PROJECT_REPORT.docx               ← detailed findings report (Word)
├── PROJECT_REPORT.md                 ← detailed findings report (Markdown)
├── README.md                         ← this file
└── output_charts/                    ← generated charts (created on run)
    ├── bq2_browser_revenue.png
    ├── bq3_country_revenue.png
    ├── bq4_membership_revenue.png
    ├── bq5_payment_method.png
    ├── bq6_gender_revenue.png
    ├── bq7_age_revenue.png
    ├── bq9_hourly_pattern.png
    ├── bq10_return_rates.png
    ├── bq11_duration_vs_sales.png
    ├── bq12_network_protocol.png
    ├── bq13_daily_revenue.png
    └── bq14_age_payment_heatmap.png
```

---

## Prerequisites

- Python **3.8** or higher
- pip (comes with Python)

---

## Quick Start

### 1. Install dependencies

```bash
pip install -r requirements.txt
```

### 2. Run the analysis

```bash
python ecommerce_analysis.py
```

The script will:
1. Load the CSV dataset
2. Clean and validate the data (print a detailed report to the console)
3. Answer 14 business questions (results printed to console)
4. Save 12 charts to `./output_charts/`

---

## Dataset Columns

| Column | Type | Description |
|---|---|---|
| `accessed_date` | datetime | UTC timestamp of the session |
| `duration_(secs)` | int | Session duration in seconds |
| `network_protocol` | string | TCP / ICMP / HTTP |
| `ip` | string | Visitor IP address |
| `bytes` | int | Data transferred (bytes) |
| `accessed_Ffom` | string | Browser or app used (typo in source — fixed in script) |
| `age` | int | Visitor age |
| `gender` | string | Male / Female |
| `country` | string | ISO 2-letter country code |
| `membership` | string | Normal / Premium |
| `language` | string | Interface language |
| `sales` | float | Order value in USD |
| `returned` | string | Yes / No – item returned |
| `returned_amount` | float | USD value of returned goods |
| `pay_method` | string | Credit Card / Debit Card / Cash / Others |

---

## Data Cleaning Highlights

| Issue | Fix |
|---|---|
| Column header typo (`accessed_Ffom`) | Renamed to `accessed_from` |
| Thousands-separator dots in `sales` / `returned_amount` (e.g. `9.575.775`) | Custom parser converts to float correctly |
| Non-positive `duration_secs` rows | Dropped |
| Duplicate rows | Dropped |
| String whitespace | Stripped across all categorical columns |
| `returned` boolean inconsistency | Standardised to Title Case (`Yes` / `No`) |

### Derived Columns Added

| New Column | Derivation |
|---|---|
| `date_only` | Date portion of `accessed_date` |
| `hour` | Hour of day (0–23) |
| `weekday` | Day name |
| `month` | Year-Month period |
| `is_returned` | Boolean derived from `returned == 'Yes'` |
| `age_group` | Binned age: `<18` · `18-24` · `25-34` · `35-44` · `45-54` · `55-64` · `65+` |

---

## Key Findings at a Glance

> Charts below are interactive — generated from the same analysis pipeline described in this README.

### 🌐 BQ-2 · Revenue by Browser / Platform

Firefox and Chrome jointly top the revenue table. iOS and Android apps are a rapidly growing mobile channel. The **Others** category (bots / scripts) inflates session count but contributes differently to revenue.

![BQ-2 Revenue by Browser](output_charts/bq2_browser_revenue.png)

---

### 🗺️ BQ-3 · Revenue by Country (Top 15)

**US → CA → IN** are the undisputed top 3 markets. European markets (DE, FR, IT) form a solid mid-tier. Emerging markets (AR, CN) show future growth potential.

![BQ-3 Country Revenue](output_charts/bq3_country_revenue.png)

---

### 💳 BQ-4 · Membership Impact on Revenue

Premium members account for a disproportionately large revenue share and exhibit more intentional (lower-return) purchasing behaviour.

![BQ-4 Membership Revenue](output_charts/bq4_membership_revenue.png)

| Metric | Normal | Premium |
|---|---|---|
| Revenue share | ~42% | ~58% |
| Avg Order Value | Lower | **Higher** |
| Return Rate | ~7% | **~5.5%** |

---

### 💰 BQ-5 · Payment Method Distribution

Credit Card is both the most-used and highest-revenue payment channel. Ensuring a friction-free credit card checkout is the single biggest lever for revenue conversion.

![BQ-5 Payment Method](output_charts/bq5_payment_method.png)

---

### 👥 BQ-6 · Gender & Buying Behaviour

Both genders contribute substantially. Female shoppers generate a higher share of total sessions; male shoppers carry a marginally higher average order value. Return rates are similar (~6–7%).

![BQ-6 Gender Revenue](output_charts/bq6_gender_revenue.png)

---

### 🎂 BQ-7 · Revenue & AOV by Age Group

The **25–34** and **35–44** cohorts dominate total revenue by volume. The **65+** cohort, despite lower session frequency, has the highest average order value — a premium niche worth separate marketing.

![BQ-7 Age Revenue](output_charts/bq7_age_revenue.png)

---

### ⏰ BQ-9 · Hourly Traffic & Revenue Pattern

Traffic and revenue peak in the **20:00–23:00 UTC** window — aligning with prime time across multiple time zones. A secondary peak occurs at **08:00–10:00 UTC**. Both windows are ideal for flash sales and push notifications.

![BQ-9 Hourly Pattern](output_charts/bq9_hourly_pattern.png)

---

### ↩️ BQ-10 · Return Rates

Return rates are broadly consistent (**5–10%**) across browsers, genders, and membership tiers. No single browser is disproportionately associated with returns. Premium members show marginally lower return rates.

![BQ-10 Return Rates](output_charts/bq10_return_rates.png)

---

### ⏱️ BQ-11 · Session Duration vs Sales

The Pearson correlation between `duration_secs` and `sales` is close to **zero**. Longer sessions do not translate to higher purchases — focus on **intent signals**, not time-on-site.

![BQ-11 Duration vs Sales](output_charts/bq11_duration_vs_sales.png)

---

### 🌐 BQ-12 · Network Protocol Distribution

TCP accounts for **>99%** of all sessions. ICMP and raw HTTP appear minimally — ICMP sessions warrant a security audit as they represent non-standard (potentially scanning) traffic.

![BQ-12 Network Protocol](output_charts/bq12_network_protocol.png)

---

### 📅 BQ-13 · Daily Revenue Trend

Revenue shows moderate day-to-day variability with mild weekly periodicity (weekends slightly lower for B2C in some regions). No dramatic growth or decline is visible in the 90-day window, indicating a **stable, mature platform**.

![BQ-13 Daily Revenue](output_charts/bq13_daily_revenue.png)

---

### 🔥 BQ-14 · Avg Sales by Age Group × Payment Method

Cash payments by **65+** customers and Credit Card usage by **35–44 year-olds** produce the highest average order values. Use this heatmap to personalise checkout prompts (e.g. surface credit card options for 35–54 age groups).

![BQ-14 Heatmap](output_charts/bq14_age_payment_heatmap.png)

---

## Business Questions Answered

| # | Question | Chart |
|---|---|---|
| BQ-1 | Overall sales metrics (revenue, AOV, return rate) | Console output |
| BQ-2 | Which browser/platform drives the most revenue? | `bq2_browser_revenue.png` |
| BQ-3 | Which country generates the most sales? | `bq3_country_revenue.png` |
| BQ-4 | How does membership type affect revenue and return rate? | `bq4_membership_revenue.png` |
| BQ-5 | Most popular and most profitable payment method? | `bq5_payment_method.png` |
| BQ-6 | How does gender influence buying behaviour? | `bq6_gender_revenue.png` |
| BQ-7 | Which age group spends the most? | `bq7_age_revenue.png` |
| BQ-8 | Which language segment is more valuable? | Console output |
| BQ-9 | What is the hourly traffic and revenue pattern? | `bq9_hourly_pattern.png` |
| BQ-10 | Return rate by browser, gender, and membership? | `bq10_return_rates.png` |
| BQ-11 | Does session duration correlate with sales? | `bq11_duration_vs_sales.png` |
| BQ-12 | Network protocol distribution | `bq12_network_protocol.png` |
| BQ-13 | Daily revenue trend | `bq13_daily_revenue.png` |
| BQ-14 | Average sales by age group × payment method (heatmap) | `bq14_age_payment_heatmap.png` |

Full findings are documented in [`PROJECT_REPORT.md`](PROJECT_REPORT.md).

---

## Key Insights & Recommendations

| # | Insight | Recommended Action |
|---|---|---|
| 1 | **Premium members** generate ~58% of total revenue | Invest in Normal → Premium conversion campaigns |
| 2 | **US, CA, IN** are the top 3 markets | Prioritise localised promotions and fast checkout for these markets |
| 3 | Peak revenue window is **20:00–23:00 UTC** | Schedule flash sales, push notifications, and email campaigns in this window |
| 4 | **Session duration ≈ zero correlation** with sales | Stop optimising for time-on-site; focus on intent-based UX signals |
| 5 | **Credit Card** dominates revenue | Ensure friction-free CC checkout; offer instalment options |
| 6 | **25–44 age group** is the core revenue driver | Personalise marketing and recommendations for this cohort |
| 7 | **65+ age group** has the highest AOV | Treat as a premium niche with targeted high-value product placement |
| 8 | **ICMP traffic** is present in logs | Security audit recommended; these should not be browsing sessions |
| 9 | **Return rate is low (~5–10%)** | Healthy baseline; monitor by SKU for further reduction |
| 10 | **Mobile apps** (iOS/Android) are growing channels | Invest in mobile UX and app-exclusive deals |
| 11 | **Spanish language** segment is the largest | Ensure full localisation (currency, language, support) for ES/LATAM markets |

---

## Dependencies

| Package | Minimum Version |
|---|---|
| pandas | 1.5.0 |
| numpy | 1.23.0 |
| matplotlib | 3.6.0 |
| seaborn | 0.12.0 |

---

## License

For educational and analytical purposes only.
