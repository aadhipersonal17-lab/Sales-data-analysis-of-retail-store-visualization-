# 📊 Retail Sales Data Analysis — FY 2024

> A full-scale retail sales data visualization and analysis report built with Python's data science stack — covering revenue trends, category performance, regional breakdowns, payment behaviour, outlier detection, and executive KPI dashboards.

---

## 🔍 Overview

This project performs end-to-end exploratory data analysis (EDA) on a retail store's transactional dataset for FY 2024, comprising approximately **10,000 transactions** totalling **₹1.84 Crore** in revenue. All insights are visualized across **10 professionally designed charts** generated using Matplotlib and Seaborn.

---

## 📁 Dataset Summary

| Metric             | Value       |
|--------------------|-------------|
| Total Revenue      | ₹1.84 Crore |
| Total Transactions | ~10,000     |
| Average Transaction| ₹1,842      |
| Median Sale        | ₹1,550      |
| Time Period        | Jan–Dec 2024|

---

## 📈 Key Analyses Performed

### 1. 📅 Monthly Revenue Trend
- Full 12-month revenue timeline with MoM growth rates
- Festive season (Oct–Dec) highlighted — **Q4 alone contributes 30.9%** of annual revenue
- Peak month: **December at ₹21.54 Lakhs**

### 2. 🛒 Revenue by Product Category
- **Electronics dominates** at ₹48.33L (30.5% share)
- Followed by Clothing (₹36.14L), Kitchen (₹24.89L), Footwear, Sports
- **Books is the weakest** category at just ₹9.41L (5.9%)

### 3. 💳 Payment Method Distribution
- **Card payments lead at 44.7%** (₹82.34L)
- Combined digital payments (Card + UPI) exceed **65% of total revenue**
- Net Banking is the smallest segment at 4.7%

### 4. 🗺️ Region-wise Revenue Performance
- **North leads** with ₹52.40L (28.3% share)
- All four regions show balanced performance — gap between top and bottom is ~22%
- **West region identified** as the primary growth opportunity

### 5. 🏆 Top 10 Products by Revenue
- **#1: 4K Smart TV** — ₹5.84 Lakhs
- **#2: Laptop Pro 15** — ₹5.12 Lakhs
- Top 3 products (all Electronics) account for ~8% of total revenue
- First non-Electronics product: Men's Leather Jacket at rank #6

### 6. 🔥 Regional × Category Heatmap
- Cross-dimensional analysis of all 4 regions × 6 categories
- **North × Electronics** is the highest-revenue cell (₹18.42L)
- Kitchen and Footwear notably strong in the **East region**

### 7. 📆 Weekly Revenue Trend
- 52-week granular view confirming macro patterns
- Growth plateau visible in **Weeks 20–30** (mid-year summer dip)
- Acceleration begins **Week 40** (October), aligned with festive demand

### 8. 🔎 Outlier Detection (3-Sigma Rule)
- Population mean: ₹1,842 | Std deviation: ₹610
- Outlier threshold (µ+3σ): **₹3,672**
- ~50 records (~0.5%) flagged — potential bulk orders, premium sales, or fraud

### 9. 📊 Quarterly Revenue & Growth Analysis

| Quarter | Period  | Revenue   | QoQ Growth | % of Annual |
|---------|---------|-----------|------------|-------------|
| Q1      | Jan–Mar | ₹25.16L   | —          | 13.7%       |
| Q2      | Apr–Jun | ₹32.04L   | +27.4%     | 17.4%       |
| Q3      | Jul–Sep | ₹35.10L   | +9.6%      | 19.0%       |
| Q4      | Oct–Dec | ₹56.95L   | +62.2%     | 30.9%       |

### 10. 🖥️ Executive Summary Dashboard
- Three KPI gauges: Electronics share (30%), Digital payments (66%), North region share (28%)
- Side-by-side H1 vs H2 monthly comparison chart
- H2 revenue is **2.3× H1 revenue**

---

## 🛠️ Tech Stack

| Tool          | Purpose                          |
|---------------|----------------------------------|
| Python 3.x    | Core programming language        |
| Pandas        | Data loading, cleaning, aggregation |
| NumPy         | Statistical computations         |
| Matplotlib    | Base chart rendering             |
| Seaborn       | Heatmap and styled visualizations|
| ReportLab     | PDF report generation            |

---

## 💡 Key Business Insights

| # | Finding                                        | Implication                                |
|---|------------------------------------------------|--------------------------------------------|
| 1 | Electronics is top revenue category (30.5%)    | Prioritize inventory and promotions        |
| 2 | Oct–Dec festive season peaks at ₹56.95L        | Increase staffing and stock in Q4          |
| 3 | Card + UPI exceed 65% of payments              | Invest in digital payment infrastructure   |
| 4 | North region leads all regions                 | Model North strategies for other regions   |
| 5 | 4K Smart TV is top-selling product             | Feature prominently in marketing           |
| 6 | ~0.5% transactions are outliers                | Implement automated fraud review system    |
| 7 | Books underperforms across all regions         | Launch targeted reading promotions         |
| 8 | H2 revenue is 2.3× H1 revenue                 | Plan promotions and logistics for H2 early |

---

## 📄 Project Structure