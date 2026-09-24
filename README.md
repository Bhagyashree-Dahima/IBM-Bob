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
├── PROJECT_REPORT.md                 ← detailed findings report
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

| Column | Description |
|---|---|
| `accessed_date` | Timestamp of the session |
| `duration_(secs)` | Session length in seconds |
| `network_protocol` | TCP / ICMP / HTTP |
| `ip` | Visitor IP address |
| `bytes` | Data transferred |
| `accessed_Ffom` | Browser or mobile app (header has a typo; fixed in script) |
| `age` | Visitor age |
| `gender` | Male / Female |
| `country` | ISO 2-letter country code |
| `membership` | Normal / Premium |
| `language` | Interface language |
| `sales` | Order value (USD) — some values use European dot-thousands format |
| `returned` | Whether the item was returned (Yes/No) |
| `returned_amount` | USD value returned |
| `pay_method` | Payment method |

---

## Data Cleaning Highlights

| Issue | Fix |
|---|---|
| Column header typo (`accessed_Ffom`) | Renamed to `accessed_from` |
| Thousands-separator dots in `sales` / `returned_amount` (e.g. `9.575.775`) | Custom parser converts to float correctly |
| Non-positive `duration_secs` rows | Dropped |
| Duplicate rows | Dropped |
| String whitespace | Stripped |
| `returned` boolean inconsistency | Standardised to Title Case |

---

## Business Questions Answered

| # | Question |
|---|---|
| BQ-1 | Overall sales metrics (revenue, AOV, return rate) |
| BQ-2 | Which browser/platform drives the most revenue? |
| BQ-3 | Which country generates the most sales? |
| BQ-4 | How does membership type affect revenue and return rate? |
| BQ-5 | Most popular and most profitable payment method? |
| BQ-6 | How does gender influence buying behaviour? |
| BQ-7 | Which age group spends the most? |
| BQ-8 | Which language segment is more valuable? |
| BQ-9 | What is the hourly traffic and revenue pattern? |
| BQ-10 | Return rate by browser, gender, and membership? |
| BQ-11 | Does session duration correlate with sales? |
| BQ-12 | Network protocol distribution |
| BQ-13 | Daily revenue trend |
| BQ-14 | Average sales by age group × payment method (heatmap) |

Full findings are documented in [`PROJECT_REPORT.md`](PROJECT_REPORT.md).

---

## Key Findings (Summary)

- **Premium members** generate significantly more revenue per session than Normal members.
- **US, CA, IN** are the top 3 markets by total revenue.
- Peak revenue window is **20:00–23:00 UTC** — ideal for flash sales.
- **Session duration has near-zero correlation with sales** — focus on intent, not time on site.
- **Credit Card** is the dominant and highest-revenue payment method.
- **25–44 age group** is the core revenue driver.

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
