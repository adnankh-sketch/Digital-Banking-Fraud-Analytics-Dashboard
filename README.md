<div align="center">

<!-- BANNER -->
<img src="https://capsule-render.vercel.app/api?type=waving&color=0E6B6B,14B8A6&height=200&section=header&text=Digital%20Banking%20Fraud%20Analytics&fontSize=36&fontColor=ffffff&fontAlignY=38&desc=Power%20BI%20Intelligence%20Dashboard&descAlignY=58&descSize=18" width="100%"/>

<br/>

<!-- SHIELDS -->
![Power BI](https://img.shields.io/badge/Power%20BI-F2C811?style=for-the-badge&logo=powerbi&logoColor=black)
![DAX](https://img.shields.io/badge/DAX-0078D4?style=for-the-badge&logo=microsoftazure&logoColor=white)
![CSV](https://img.shields.io/badge/Data-CSV-14B8A6?style=for-the-badge&logo=databricks&logoColor=white)
![Status](https://img.shields.io/badge/Status-Active-0E6B6B?style=for-the-badge)
![License](https://img.shields.io/badge/License-MIT-teal?style=for-the-badge)

<br/>

> **A production-grade Power BI dashboard for detecting fraud patterns, segmenting customers, and delivering executive-level banking intelligence — built on a star-schema data model with custom DAX measures.**

<br/>

[📊 Dashboard Overview](#-dashboard-overview) · [🗃️ Data Model](#️-data-model) · [📐 DAX Measures](#-dax-measures) · [🎨 Design System](#-design-system) · [🚀 Getting Started](#-getting-started)

</div>

---

## 📌 Table of Contents

- [📊 Dashboard Overview](#-dashboard-overview)
- [🗃️ Data Model](#️-data-model)
- [📐 DAX Measures](#-dax-measures)
- [📋 Report Pages](#-report-pages)
- [🎨 Design System](#-design-system)
- [🚀 Getting Started](#-getting-started)
- [📁 Repository Structure](#-repository-structure)
- [🤝 Contributing](#-contributing)

---

## 📊 Dashboard Overview

The **Digital Banking Fraud Analytics Dashboard** is a three-page interactive Power BI report built for financial institutions to monitor transaction health, identify fraud risk, and understand customer behaviour — all through a clean monochromatic teal design language.

<div align="center">

| 🏦 Executive Overview | 👥 Customer Segmentation | 🚨 Risk & Fraud Monitor |
|:---:|:---:|:---:|
| High-level KPIs & trends | Segment-level spend analysis | Fraud detection & risk scoring |

</div>

### ✨ Key Capabilities

- 🔍 **Fraud Pattern Detection** — Surface suspicious transaction clusters by type, merchant, and time
- 💡 **Executive KPIs** — Total revenue, transaction volume, fraud rate, and average transaction value
- 👤 **Customer Segmentation** — Cohort analysis across spend behaviour and risk tier
- 📈 **Time-Series Trends** — Daily/monthly transaction volume with drill-through capability
- 🔗 **Star-Schema Model** — Fully normalised fact + dimension table architecture
- 🎨 **Teal Design Theme** — Consistent monochromatic palette (`#0E6B6B` → `#14B8A6`)

---

## 🗃️ Data Model

The report is powered by **four linked CSV files** forming a classic star schema:

```
                    ┌─────────────────────┐
                    │  fact_transactions  │
                    │   (Fact Table)      │
                    │─────────────────────│
                    │ TransactionID  (PK) │
                    │ CustomerID     (FK) │──────────► dim_customer
                    │ MerchantCatID  (FK) │──────────► dim_merchant_category
                    │ TransTypeID    (FK) │──────────► dim_transaction_type
                    │ Amount              │
                    │ TransactionDate     │
                    │ IsFraud             │
                    └─────────────────────┘
```

### 📂 Source Files

| File | Type | Description |
|------|------|-------------|
| `fact_transactions_updated.csv` | Fact | Core transaction ledger with amounts, dates, and fraud flags |
| `dim_customer.csv` | Dimension | Customer profiles, segments, and risk tiers |
| `dim_merchant_category.csv` | Dimension | Merchant categories and industry classifications |
| `dim_transaction_type.csv` | Dimension | Transaction type labels (debit, credit, transfer, etc.) |

### 🔗 Relationships

```
dim_customer          [CustomerID]    ──1:M──  fact_transactions [CustomerID]
dim_merchant_category [MerchantCatID] ──1:M──  fact_transactions [MerchantCatID]
dim_transaction_type  [TransTypeID]   ──1:M──  fact_transactions [TransTypeID]
```

> All relationships use **single-direction filtering** from dimension → fact to maintain clean measure propagation.

---

## 📐 DAX Measures

All measures are organised in a dedicated `_Measures` table for clean report navigation.

### 💰 Core KPIs

```dax
-- Total Transaction Value
Total Revenue =
SUMX(
    fact_transactions_updated,
    fact_transactions_updated[Amount]
)

-- Transaction Count
Total Transactions =
COUNTROWS(fact_transactions_updated)

-- Average Transaction Value
Avg Transaction Value =
DIVIDE([Total Revenue], [Total Transactions], 0)
```

### 🚨 Fraud Metrics

```dax
-- Fraud Transaction Count
Fraud Count =
CALCULATE(
    COUNTROWS(fact_transactions_updated),
    fact_transactions_updated[IsFraud] = 1
)

-- Fraud Rate %
Fraud Rate % =
DIVIDE([Fraud Count], [Total Transactions], 0)

-- Fraud Loss Amount
Fraud Amount =
CALCULATE(
    [Total Revenue],
    fact_transactions_updated[IsFraud] = 1
)
```

### 📅 Time Intelligence

```dax
-- Month-over-Month Volume Change
MoM Transaction Change % =
VAR CurrentMonth = [Total Transactions]
VAR PrevMonth =
    CALCULATE(
        [Total Transactions],
        DATEADD(dim_date[Date], -1, MONTH)
    )
RETURN
    DIVIDE(CurrentMonth - PrevMonth, PrevMonth, 0)
```

---

## 📋 Report Pages

### 1️⃣ Executive Overview
> **Audience:** C-Suite / Senior Management

Delivers a single-glance financial health summary with:
- **KPI Cards** — Total Revenue · Transactions · Avg Value · Fraud Rate %
- **Line Chart** — Daily transaction volume over time (date-axis with continuous scale)
- **Bar Chart** — Revenue breakdown by merchant category
- **Donut Chart** — Transaction type distribution

---

### 2️⃣ Customer Segmentation
> **Audience:** Risk & Marketing Teams

Breaks down customer behaviour across segments:
- **Clustered Bar** — Spend by customer segment and transaction type
- **Scatter Plot** — Customer frequency vs. average spend
- **Table Visual** — Top customers by transaction volume with risk tier
- **Slicer Panel** — Filter by segment, date range, and transaction type

---

### 3️⃣ Risk & Fraud Monitor
> **Audience:** Fraud Analysts / Compliance

Surfaces fraud signals with actionable drill-through:
- **KPI Cards** — Fraud Count · Fraud Amount · Fraud Rate %
- **Column Chart** — Fraud volume by merchant category
- **Matrix Table** — Transaction type × fraud flag heat map
- **Flagged Transactions Table** — Drill-through list with IsFraud = 1 filter

---

## 🎨 Design System

The report uses a **monochromatic teal** palette for a clean, professional look:

<div align="center">

| Role | Hex | Usage |
|------|-----|-------|
| 🟦 Primary Dark | `#0E6B6B` | Headers, key KPI cards |
| 🟩 Primary | `#0F8B8B` | Chart primary series |
| 🌊 Accent | `#14B8A6` | Highlights, hover states |
| 💧 Light | `#99D4CE` | Secondary chart series |
| ⬜ Background | `#F0F9F9` | Canvas background |
| ⬛ Text | `#1A1A2E` | Body text & labels |

</div>

### 🖋️ Typography
- **Titles:** Segoe UI Semibold · 18–20pt
- **KPI Values:** Segoe UI Bold · 28–32pt
- **Labels / Subtitles:** Segoe UI · 11–13pt

---

## 🚀 Getting Started

### Prerequisites

- [Microsoft Power BI Desktop](https://powerbi.microsoft.com/desktop/) (latest version recommended)
- The four CSV source files (included in `/data/`)

### Installation

```bash
# 1. Clone the repository
git clone https://github.com/adnankh-sketch/Digital-Banking-Fraud-Analytics-Dashboard.git

# 2. Navigate to the project folder
cd Digital-Banking-Fraud-Analytics-Dashboard
```

### Open in Power BI

1. Launch **Power BI Desktop**
2. Open `Digital_Banking_Fraud_Analytics.pbix`
3. If prompted, update the data source paths to your local `/data/` folder:
   - `Home` → `Transform Data` → `Data Source Settings`
4. Click **Refresh** to load the latest data
5. Explore all three report pages from the bottom navigation tabs

---

## 📁 Repository Structure

```
Digital-Banking-Fraud-Analytics-Dashboard/
│
├── 📊 Digital_Banking_Fraud_Analytics.pbix   # Main Power BI report file
│
├── 📂 data/
│   ├── fact_transactions_updated.csv         # Core transaction fact table
│   ├── dim_customer.csv                      # Customer dimension
│   ├── dim_merchant_category.csv             # Merchant category dimension
│   └── dim_transaction_type.csv              # Transaction type dimension
│
├── 📂 screenshots/
│   ├── 01_executive_overview.png
│   ├── 02_customer_segmentation.png
│   └── 03_risk_fraud_monitor.png
│
└── 📄 README.md
```

---

## 🤝 Contributing

Contributions, issues, and feature requests are welcome!

1. **Fork** the repository
2. Create your feature branch: `git checkout -b feature/new-measure`
3. Commit your changes: `git commit -m 'Add: new fraud clustering measure'`
4. Push to the branch: `git push origin feature/new-measure`
5. Open a **Pull Request**

---

## 📄 License

This project is licensed under the **MIT License** — see the [LICENSE](LICENSE) file for details.

---

<div align="center">

**Built with ❤️ using Microsoft Power BI**

![Power BI](https://img.shields.io/badge/Power%20BI-F2C811?style=flat-square&logo=powerbi&logoColor=black)
![DAX](https://img.shields.io/badge/DAX-0078D4?style=flat-square&logo=microsoftazure&logoColor=white)
![Star Schema](https://img.shields.io/badge/Architecture-Star%20Schema-14B8A6?style=flat-square)

<br/>

*If this project helped you, give it a ⭐ on GitHub!*

<img src="https://capsule-render.vercel.app/api?type=waving&color=0E6B6B,14B8A6&height=100&section=footer" width="100%"/>

</div>
