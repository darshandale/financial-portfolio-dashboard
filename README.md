# 📊 Executive Wealth & Portfolio Insights Dashboard

## 🎯 Project Overview

**Executive Wealth & Portfolio Insights Dashboard** is an end-to-end investment portfolio analytics project that simulates and analyzes a diversified Indian investment portfolio consisting of:

* **5 large-cap equities**
* **5 ETFs / mutual fund-style holdings**
* **Nifty 50 benchmark**

The project demonstrates how raw market data and transaction records can be transformed into a structured analytical data model and finally into an **executive-level Business Intelligence dashboard**.

The workflow follows a real-world analytics architecture:


Market Data + Transaction Data
            ↓
      Python Data Pipeline
            ↓
   Data Cleaning & Transformation
            ↓
        Star Schema
            ↓
        CSV Data Layer
            ↓
       Excel BI Layer
            ↓
 PivotTables + PivotCharts + KPIs
            ↓
 Executive Portfolio Insights


---

## 💼 Business Objective

The objective is to answer practical portfolio-management questions such as:

* How much capital has been invested?
* What is the current portfolio value?
* Which assets are generating gains or losses?
* Which sectors have the highest exposure?
* How is capital distributed between equities and ETFs?
* Which holdings contribute most to portfolio performance?
* How much has been paid in transaction fees?
* How has investment activity changed over time?
* How does portfolio performance compare with the Nifty 50 benchmark?

---

## 🏗️ Project Architecture

The project follows a **two-layer architecture**.

### 1️⃣ Data Layer — Python

Python is responsible for:

* Retrieving historical market prices using `yfinance`
* Generating a reproducible simulated transaction ledger
* Structuring data into a star-schema model
* Performing data preparation and transformation
* Exporting analytical tables to CSV

### 2️⃣ Presentation Layer — Excel

Excel is used as the Business Intelligence layer for:

* KPI reporting
* PivotTables
* PivotCharts
* Portfolio allocation analysis
* Sector diversification
* Investment activity trends
* Holdings-level performance analysis
* Conditional formatting and executive reporting

---

## 📐 Data Model

The project uses a simplified **star schema**.

```text
                  ┌──────────────────────┐
                  │      dim_assets      │
                  ├──────────────────────┤
                  │ AssetID              │
                  │ Ticker               │
                  │ Name                 │
                  │ Class                │
                  │ Sector               │
                  │ BenchmarkTicker      │
                  └──────────┬───────────┘
                             │
                ┌────────────┴────────────┐
                │                         │
                ▼                         ▼
      ┌─────────────────────┐   ┌────────────────────────┐
      │ fact_daily_prices   │   │   fact_transactions    │
      ├─────────────────────┤   ├────────────────────────┤
      │ Date                │   │ TransactionID          │
      │ AssetID             │   │ Date                   │
      │ ClosePrice          │   │ AssetID                │
      │                     │   │ TransactionType        │
      │ AssetID = 0         │   │ Units                  │
      │ = Nifty 50          │   │ PricePerUnit           │
      └─────────────────────┘   │ Fees                   │
                                │ TotalAmount            │
                                └────────────────────────┘
```

### Tables

| Table               | Purpose                           |
| ------------------- | --------------------------------- |
| `dim_assets`        | Asset master/dimension table      |
| `fact_daily_prices` | Historical daily market prices    |
| `fact_transactions` | Simulated investment transactions |

---

## 📊 Portfolio Universe

The simulated portfolio contains:

### Equities

* Large-cap Indian equity holdings
* Sector-based diversification
* Discretionary BUY/SELL transactions

### ETFs

* Diversified ETF exposure
* Monthly SIP-style investments
* Exposure across gold, broad market, international technology, small-cap and mid-cap themes

### Benchmark

* **Nifty 50 (`^NSEI`)**

The benchmark is maintained separately in the price fact table using:

```text
AssetID = 0
```

---

## 🐍 Data Generation Pipeline

The main data-generation script is:

```text
scripts/generate_data.py
```

The script performs the following steps:

```text
1. Load asset configuration
        ↓
2. Download historical market prices
        ↓
3. Prepare daily price data
        ↓
4. Generate monthly ETF SIP transactions
        ↓
5. Generate equity BUY/SELL transactions
        ↓
6. Calculate transaction values and fees
        ↓
7. Apply reproducibility seed
        ↓
8. Export CSV datasets
```

### Historical Market Data

The pipeline retrieves daily closing prices for:

```text
2022-01-01 → 2026-06-01
```

using `yfinance`.

### Reproducibility

Randomized transactions are generated using:

```python
np.random.seed(42)
```

This ensures that the simulated transaction dataset can be reproduced consistently.

---

## 💰 Transaction Simulation

The transaction ledger contains two primary investment behaviors.

### ETF Investments

Monthly SIP-style investments are generated for the ETF holdings.

Investment amounts are randomized within a predefined range:

```text
₹2,500 – ₹10,000
```

### Equity Transactions

Equity transactions simulate discretionary portfolio activity using randomized BUY/SELL behavior.

The transaction distribution is approximately:

```text
75% → Small BUY
15% → Larger BUY
5%  → Bigger BUY
5%  → SELL
```

Each transaction contains:

* Transaction ID
* Date
* Asset
* Transaction type
* Units
* Price per unit
* Fees
* Total transaction amount

---

# 📈 Executive Dashboard

The Excel dashboard converts the structured datasets into an executive-level portfolio monitoring interface.

## KPI Cards

The dashboard tracks:

| KPI                      | Description                                 |
| ------------------------ | ------------------------------------------- |
| 💰 Total Portfolio Value | Current market value of holdings            |
| 💵 Net Invested Capital  | Capital invested after transaction activity |
| 📈 Unrealized Gain/Loss  | Current unrealized portfolio P&L            |
| 📊 Total Return %        | Portfolio return percentage                 |
| 💸 Cumulative Fees       | Total transaction fees paid                 |
| 🏦 Active Assets         | Number of assets currently held             |

---

## 🍩 Asset Class Allocation

A donut chart displays portfolio exposure across:

```text
Equity
vs.
Mutual Fund / ETF
```

This provides a quick view of the portfolio's asset-class concentration.

---

## 🏭 Sector Diversification

The dashboard analyzes exposure across eight sector/theme categories:

* Financials
* Technology
* Energy
* Commodities
* International Tech
* Small Cap
* Broad Market
* Mid Cap

This helps identify concentration and diversification patterns.

---

## 📅 Investment Activity & Fee Progression

A combination chart tracks:

* Monthly investment volume
* Transaction fees
* Investment activity over time

This provides visibility into how capital deployment and transaction costs evolve throughout the investment period.

---

## 📋 Holdings Performance Table

Each asset is evaluated using:

| Metric           | Description                       |
| ---------------- | --------------------------------- |
| Net Units        | Current units held                |
| Avg Cost / Unit  | Average acquisition cost          |
| Current Price    | Latest available market price     |
| Cost Basis       | Total acquisition cost            |
| Current Value    | Current market value              |
| Unrealized P&L   | Current unrealized gain/loss      |
| Return %         | Holding-level return              |
| Portfolio Weight | Contribution to portfolio         |
| Gain/Loss Flag   | Conditional performance indicator |

---

# 🔎 Current Portfolio Insights

At the latest dashboard refresh, the simulated portfolio shows:

### Overall Performance

```text
Portfolio Return: -8.04%
```

The portfolio is currently below its aggregate invested cost basis.

### Sector Concentration

Technology represents approximately:

```text
40.8%
```

of portfolio exposure, primarily driven by the two technology equity holdings.

This makes Technology the largest sector exposure in the simulated portfolio.

### Equity Performance

The two technology holdings are currently among the largest contributors to the portfolio's unrealized losses.

### ETF Performance

All five ETF positions are currently profitable.

However, their combined portfolio weight is approximately:

```text
8.3%
```

which limits their ability to offset the larger losses from the equity holdings.

> **Important:** These figures describe a simulated portfolio generated for analytics and demonstration purposes. They should not be interpreted as actual investment performance or financial advice.

---
 **End-to-end multi-asset investment portfolio analytics system built with Python, financial market data, and Excel BI — designed to demonstrate data engineering, financial analytics, data modeling, and executive dashboarding skills.**

[![Python](https://img.shields.io/badge/Python-3.x-3776AB?logo=python\&logoColor=white)](https://www.python.org/)
[![Pandas](https://img.shields.io/badge/Pandas-Data%20Analysis-150458?logo=pandas\&logoColor=white)](https://pandas.pydata.org/)
[![NumPy](https://img.shields.io/badge/NumPy-Data%20Processing-013243?logo=numpy\&logoColor=white)](https://numpy.org/)
[![yfinance](https://img.shields.io/badge/yfinance-Market%20Data-2E7D32)](https://github.com/ranaroussi/yfinance)
[![Excel](https://img.shields.io/badge/Excel-BI%20Dashboard-217346?logo=microsoftexcel\&logoColor=white)](https://www.microsoft.com/microsoft-365/excel)
[![License](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

---


# 🛠️ Technology Stack

| Technology                | Purpose                                          |
| ------------------------- | ------------------------------------------------ |
| 🐍 Python                 | Data generation and transformation               |
| 🐼 Pandas                 | Data manipulation and analysis                   |
| 🔢 NumPy                  | Numerical operations and reproducible simulation |
| 📈 yfinance               | Historical market price retrieval                |
| 📊 Microsoft Excel        | Business Intelligence dashboard                  |
| 🔄 PivotTables            | Aggregation and analysis                         |
| 📈 PivotCharts            | Data visualization                               |
| 🎨 Conditional Formatting | Performance indicators                           |
| 🗂️ CSV                   | Analytical data storage                          |
| 🔀 Git/GitHub             | Version control and project sharing              |

---

# 📁 Repository Structure

```text
executive-wealth-portfolio-insights/
│
├── README.md
├── LICENSE
├── requirements.txt
├── .gitignore
│
├── scripts/
│   └── generate_data.py
│
├── data/
│   └── raw/
│       ├── dim_assets.csv
│       ├── fact_daily_prices.csv
│       └── fact_transactions.csv
│
├── dashboard/
│   └── Executive_Wealth_Portfolio_Dashboard.xlsx
│
├── docs/
│   └── data_dictionary.md
│
└── assets/
    └── dashboard_screenshot.png
```

---

# ▶️ How to Run the Project

## 1. Clone the repository

```bash
git clone https://github.com/YOUR_USERNAME/executive-wealth-portfolio-insights.git
```

Navigate into the project:

```bash
cd executive-wealth-portfolio-insights
```

---

## 2. Install dependencies

Create a virtual environment if desired:

```bash
python -m venv venv
```

Activate it on Windows:

```bash
venv\Scripts\activate
```

Install the required packages:

```bash
pip install -r requirements.txt
```

---

## 3. Generate the datasets

Run:

```bash
python scripts/generate_data.py
```

The script generates the CSV files inside:

```text
data/raw/
```

---

## 4. Open the dashboard

Open:

```text
dashboard/Executive_Wealth_Portfolio_Dashboard.xlsx
```

Refresh the PivotTables/PivotCharts if required.

---

# 📊 Dashboard Preview

![Executive Wealth & Portfolio Insights Dashboard](assets/dashboard_screenshot.png)

---

# 🧠 Key Data Analytics Concepts Demonstrated

This project demonstrates practical experience with:

### Data Engineering

* ETL pipeline design
* Data extraction
* Data transformation
* Structured data modeling
* Star schema design
* Reproducible data generation
* CSV-based data pipelines

### Data Analytics

* KPI development
* Portfolio performance analysis
* Asset allocation
* Sector diversification
* Profit/loss analysis
* Trend analysis
* Transaction analysis
* Benchmark analysis

### Business Intelligence

* Executive dashboards
* PivotTables
* PivotCharts
* KPI cards
* Conditional formatting
* Interactive analytical reporting

### Financial Analytics

* Cost basis
* Portfolio value
* Unrealized P&L
* Return %
* Portfolio weighting
* Investment activity
* Transaction fees
* Benchmark comparison

---

# 🚀 Future Enhancements

The project can be extended into a more advanced portfolio analytics platform.

### Advanced Portfolio Metrics

* Sharpe Ratio
* Sortino Ratio
* Annualized Volatility
* Maximum Drawdown
* CAGR
* XIRR
* Beta
* Alpha
* Tracking Error

### Advanced Visualization

* Monthly return heatmap
* Risk vs Return scatter plot
* Benchmark comparison chart
* Drawdown chart
* Rolling volatility
* Rolling Sharpe ratio

### BI Expansion

* Power BI version
* DAX-based financial measures
* Interactive slicers
* Drill-through analysis
* Executive portfolio summary page

### Application Layer

A future version could include a Streamlit-based web application with:

* Interactive portfolio filters
* Asset-level analysis
* Risk analytics
* Benchmark comparison
* Portfolio alerts
* Dynamic charts

---

# 🎓 Learning Outcomes

Through this project, I developed practical experience in:

* Designing an end-to-end analytics workflow
* Working with financial market datasets
* Building a star-schema data model
* Creating reproducible synthetic transaction data
* Transforming raw data into analytical datasets
* Developing executive-level BI dashboards
* Communicating data-driven business insights
* Applying financial analytics concepts to a realistic use case

---

# ⚠️ Disclaimer

This project is created for **educational, portfolio, and demonstration purposes only**.

The transactions are simulated and do not represent actual personal investment holdings.

Historical market data is used for analytical demonstration and does not guarantee future investment performance.

Nothing in this repository constitutes financial, investment, or trading advice.

---

# 📄 License

This project is licensed under the **MIT License**.

See the [LICENSE](LICENSE) file for details.

---

## 👨‍💻 Author

**Darshan Dale**

Final-Year Information Technology Engineering Student
Aspiring Data Analyst | Business Analytics | Financial Analytics

🔗 **GitHub:** [github.com/darshandale](https://github.com/darshandale)

🔗 **LinkedIn:** [linkedin.com/in/darshan-dale](https://linkedin.com/in/darshan-dale)

---

⭐ If you find this project useful, consider giving the repository a star!
