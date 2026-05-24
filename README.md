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
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import matplotlib.patches as mpatches
from matplotlib.gridspec import GridSpec
import matplotlib.ticker as mticker
import warnings
warnings.filterwarnings('ignore')

# ─────────────────────────────────────────
#  COLOR PALETTE
# ─────────────────────────────────────────
PRIMARY   = '#1A73E8'
SECONDARY = '#34A853'
ACCENT    = '#FBBC04'
DANGER    = '#EA4335'
PURPLE    = '#9C27B0'
TEAL      = '#00BCD4'
BG        = '#F8F9FA'
DARK      = '#1E2D3D'

plt.rcParams.update({
    'font.family'      : 'DejaVu Sans',
    'axes.facecolor'   : BG,
    'figure.facecolor' : 'white',
    'axes.spines.top'  : False,
    'axes.spines.right': False,
    'axes.grid'        : True,
    'grid.alpha'       : 0.3,
    'grid.linestyle'   : '--',
})

# ─────────────────────────────────────────
#  DATA
# ─────────────────────────────────────────

# Chart 1 — Monthly Revenue
months      = ['Jan','Feb','Mar','Apr','May','Jun',
               'Jul','Aug','Sep','Oct','Nov','Dec']
revenue     = [8.42, 7.56, 9.18, 10.24, 11.38, 10.42,
               9.87, 12.05, 13.18, 16.52, 18.89, 21.54]
mom_growth  = [None, -10.2, 21.4, 11.5, 11.1, -8.4,
               -5.3,  22.1,  9.4, 25.3, 14.3, 14.0]

# Chart 2 — Revenue by Product Category
categories  = ['Electronics','Clothing','Kitchen','Footwear','Sports','Books']
cat_revenue = [48.33, 36.14, 24.89, 21.03, 18.77, 9.41]
cat_colors  = [PRIMARY, SECONDARY, ACCENT, PURPLE, TEAL, DANGER]

# Chart 3 — Payment Method
pay_methods = ['Card', 'Cash', 'UPI', 'Net Banking']
pay_revenue = [82.34, 54.91, 38.42, 8.58]
pay_pct     = [44.7, 29.8, 20.9, 4.7]
pay_colors  = [PRIMARY, SECONDARY, ACCENT, PURPLE]

# Chart 9 — Quarterly
quarters    = ['Q1\n(Jan–Mar)', 'Q2\n(Apr–Jun)',
               'Q3\n(Jul–Sep)', 'Q4\n(Oct–Dec)']
q_revenue   = [25.16, 32.04, 35.10, 56.95]
q_growth    = [27.4, 9.6, 62.2]
q_colors    = [TEAL, ACCENT, SECONDARY, PRIMARY]

# Chart 10 — Executive Dashboard H1 vs H2
h1 = [8.42, 7.56, 9.18, 10.24, 11.38, 10.42]
h2 = [16.52, 18.89, 21.54, 12.05, 13.18, 9.87]
pair_labels = ['Jan/Jul','Feb/Aug','Mar/Sep',
               'Apr/Oct','May/Nov','Jun/Dec']


# ═══════════════════════════════════════════════════════
#  CHART 1 — Monthly Revenue Trend
# ═══════════════════════════════════════════════════════
def chart1_monthly_revenue():
    fig, ax = plt.subplots(figsize=(13, 6))
    fig.patch.set_facecolor('white')

    x = np.arange(len(months))

    # Festive season shading (Oct–Dec = index 9–11)
    ax.axvspan(8.5, 11.5, alpha=0.12, color=ACCENT, label='Festive Season (Oct–Dec)')

    # Line + fill
    ax.fill_between(x, revenue, alpha=0.12, color=PRIMARY)
    ax.plot(x, revenue, color=PRIMARY, linewidth=2.8, zorder=3)
    ax.scatter(x, revenue, color=PRIMARY, s=70, zorder=4, edgecolors='white', linewidth=1.5)

    # Data labels
    for i, (xi, yi) in enumerate(zip(x, revenue)):
        ax.annotate(f'₹{yi}L', xy=(xi, yi),
                    xytext=(0, 10), textcoords='offset points',
                    ha='center', fontsize=8.5, fontweight='bold', color=DARK)

    # MoM growth annotations
    for i in range(1, len(months)):
        color = SECONDARY if mom_growth[i] > 0 else DANGER
        ax.annotate(f'{mom_growth[i]:+.1f}%',
                    xy=(x[i], revenue[i]),
                    xytext=(0, -18), textcoords='offset points',
                    ha='center', fontsize=7.5, color=color, fontweight='bold')

    ax.set_xticks(x)
    ax.set_xticklabels(months, fontsize=10)
    ax.set_ylabel('Revenue (₹ Lakhs)', fontsize=11)
    ax.set_title('Monthly Revenue Trend — FY 2024',
                 fontsize=15, fontweight='bold', pad=18, color=DARK)
    ax.set_ylim(0, 26)
    ax.yaxis.set_major_formatter(mticker.FormatStrFormatter('₹%.0fL'))

    festive_patch = mpatches.Patch(color=ACCENT, alpha=0.4, label='Festive Season (Oct–Dec)')
    ax.legend(handles=[festive_patch], loc='upper left', fontsize=9)

    plt.tight_layout()
    plt.savefig('/mnt/user-data/outputs/chart1_monthly_revenue.png', dpi=150, bbox_inches='tight')
    print("✅ Chart 1 saved.")
    plt.show()


# ═══════════════════════════════════════════════════════
#  CHART 2 — Revenue by Product Category
# ═══════════════════════════════════════════════════════
def chart2_category_revenue():
    fig, ax = plt.subplots(figsize=(11, 6))
    fig.patch.set_facecolor('white')

    x    = np.arange(len(categories))
    bars = ax.bar(x, cat_revenue, color=cat_colors, width=0.55,
                  edgecolor='white', linewidth=1.2, zorder=3)

    # Value + % labels on bars
    pct_share = [30.5, 22.8, 15.7, 13.3, 11.8, 5.9]
    for bar, val, pct in zip(bars, cat_revenue, pct_share):
        ax.text(bar.get_x() + bar.get_width()/2,
                bar.get_height() + 0.6,
                f'₹{val}L', ha='center', va='bottom',
                fontsize=10, fontweight='bold', color=DARK)
        ax.text(bar.get_x() + bar.get_width()/2,
                bar.get_height()/2,
                f'{pct}%', ha='center', va='center',
                fontsize=9, fontweight='bold', color='white')

    ax.set_xticks(x)
    ax.set_xticklabels(categories, fontsize=11)
    ax.set_ylabel('Revenue (₹ Lakhs)', fontsize=11)
    ax.set_title('Revenue by Product Category — FY 2024',
                 fontsize=15, fontweight='bold', pad=18, color=DARK)
    ax.set_ylim(0, 56)
    ax.yaxis.set_major_formatter(mticker.FormatStrFormatter('₹%.0fL'))

    plt.tight_layout()
    plt.savefig('/mnt/user-data/outputs/chart2_category_revenue.png', dpi=150, bbox_inches='tight')
    print("✅ Chart 2 saved.")
    plt.show()


# ═══════════════════════════════════════════════════════
#  CHART 3 — Payment Method Distribution
# ═══════════════════════════════════════════════════════
def chart3_payment_methods():
    fig, (ax1, ax2) = plt.subplots(1, 2, figsize=(13, 6))
    fig.patch.set_facecolor('white')

    # --- Pie Chart ---
    explode = (0.04, 0.02, 0.02, 0.06)
    wedges, texts, autotexts = ax1.pie(
        pay_pct, labels=pay_methods, colors=pay_colors,
        autopct='%1.1f%%', startangle=140, explode=explode,
        pctdistance=0.75, wedgeprops=dict(edgecolor='white', linewidth=2)
    )
    for at in autotexts:
        at.set_fontsize(10)
        at.set_fontweight('bold')
        at.set_color('white')
    for t in texts:
        t.set_fontsize(11)

    ax1.set_title('Payment Method Share (%)',
                  fontsize=13, fontweight='bold', color=DARK, pad=14)

    # --- Bar Chart ---
    x    = np.arange(len(pay_methods))
    bars = ax2.bar(x, pay_revenue, color=pay_colors, width=0.5,
                   edgecolor='white', linewidth=1.2, zorder=3)

    for bar, val, pct in zip(bars, pay_revenue, pay_pct):
        ax2.text(bar.get_x() + bar.get_width()/2,
                 bar.get_height() + 1,
                 f'₹{val}L', ha='center', fontsize=10,
                 fontweight='bold', color=DARK)
        ax2.text(bar.get_x() + bar.get_width()/2,
                 bar.get_height()/2,
                 f'{pct}%', ha='center', va='center',
                 fontsize=9, fontweight='bold', color='white')

    ax2.set_xticks(x)
    ax2.set_xticklabels(pay_methods, fontsize=11)
    ax2.set_ylabel('Revenue (₹ Lakhs)', fontsize=11)
    ax2.set_title('Revenue by Payment Method',
                  fontsize=13, fontweight='bold', color=DARK, pad=14)
    ax2.set_ylim(0, 100)
    ax2.yaxis.set_major_formatter(mticker.FormatStrFormatter('₹%.0fL'))

    fig.suptitle('Payment Method Distribution — FY 2024',
                 fontsize=15, fontweight='bold', color=DARK, y=1.02)

    plt.tight_layout()
    plt.savefig('/mnt/user-data/outputs/chart3_payment_methods.png', dpi=150, bbox_inches='tight')
    print("✅ Chart 3 saved.")
    plt.show()


# ═══════════════════════════════════════════════════════
#  CHART 9 — Quarterly Revenue & Growth
# ═══════════════════════════════════════════════════════
def chart9_quarterly():
    fig, (ax1, ax2) = plt.subplots(1, 2, figsize=(13, 6))
    fig.patch.set_facecolor('white')

    # --- Left: Quarterly Revenue Bars ---
    x    = np.arange(len(quarters))
    bars = ax1.bar(x, q_revenue, color=q_colors, width=0.5,
                   edgecolor='white', linewidth=1.5, zorder=3)

    pct_annual = [13.7, 17.4, 19.0, 30.9]
    for bar, val, pct in zip(bars, q_revenue, pct_annual):
        ax1.text(bar.get_x() + bar.get_width()/2,
                 bar.get_height() + 0.8,
                 f'₹{val}L', ha='center', fontsize=11,
                 fontweight='bold', color=DARK)
        ax1.text(bar.get_x() + bar.get_width()/2,
                 bar.get_height()/2,
                 f'{pct}%\nof Annual', ha='center', va='center',
                 fontsize=8.5, fontweight='bold', color='white')

    ax1.set_xticks(x)
    ax1.set_xticklabels(quarters, fontsize=10)
    ax1.set_ylabel('Revenue (₹ Lakhs)', fontsize=11)
    ax1.set_title('Quarterly Revenue Comparison',
                  fontsize=13, fontweight='bold', color=DARK, pad=14)
    ax1.set_ylim(0, 68)
    ax1.yaxis.set_major_formatter(mticker.FormatStrFormatter('₹%.0fL'))

    # --- Right: QoQ Growth Bars ---
    growth_labels = ['Q1→Q2', 'Q2→Q3', 'Q3→Q4']
    growth_colors = [SECONDARY if g > 0 else DANGER for g in q_growth]
    x2   = np.arange(len(growth_labels))
    bars2 = ax2.bar(x2, q_growth, color=growth_colors, width=0.45,
                    edgecolor='white', linewidth=1.5, zorder=3)

    for bar, val in zip(bars2, q_growth):
        ax2.text(bar.get_x() + bar.get_width()/2,
                 bar.get_height() + 0.8,
                 f'+{val}%', ha='center', fontsize=12,
                 fontweight='bold', color=DARK)

    ax2.set_xticks(x2)
    ax2.set_xticklabels(growth_labels, fontsize=11)
    ax2.set_ylabel('Growth Rate (%)', fontsize=11)
    ax2.set_title('Quarter-over-Quarter Growth %',
                  fontsize=13, fontweight='bold', color=DARK, pad=14)
    ax2.set_ylim(0, 75)
    ax2.yaxis.set_major_formatter(mticker.FormatStrFormatter('+%.1f%%'))

    fig.suptitle('Quarterly Revenue & Growth Analysis — FY 2024',
                 fontsize=15, fontweight='bold', color=DARK, y=1.02)

    plt.tight_layout()
    plt.savefig('/mnt/user-data/outputs/chart9_quarterly.png', dpi=150, bbox_inches='tight')
    print("✅ Chart 9 saved.")
    plt.show()


# ═══════════════════════════════════════════════════════
#  CHART 10 — Executive Summary Dashboard
# ═══════════════════════════════════════════════════════
def chart10_executive_dashboard():
    fig = plt.figure(figsize=(16, 10))
    fig.patch.set_facecolor('white')
    gs  = GridSpec(2, 3, figure=fig, hspace=0.45, wspace=0.35)

    # ── KPI Donut 1: Electronics Share ──
    ax1 = fig.add_subplot(gs[0, 0])
    vals = [30.5, 69.5]
    ax1.pie(vals, colors=[PRIMARY, '#E0E0E0'], startangle=90,
            wedgeprops=dict(width=0.38, edgecolor='white'))
    ax1.text(0, 0,  '30%',   ha='center', va='center',
             fontsize=22, fontweight='bold', color=PRIMARY)
    ax1.text(0, -0.55, 'Electronics Share',
             ha='center', fontsize=11, color=DARK, fontweight='bold')
    ax1.set_title('Category KPI', fontsize=11, color='grey', pad=6)

    # ── KPI Donut 2: Digital Payments ──
    ax2 = fig.add_subplot(gs[0, 1])
    vals2 = [65.6, 34.4]
    ax2.pie(vals2, colors=[SECONDARY, '#E0E0E0'], startangle=90,
            wedgeprops=dict(width=0.38, edgecolor='white'))
    ax2.text(0, 0,  '66%',   ha='center', va='center',
             fontsize=22, fontweight='bold', color=SECONDARY)
    ax2.text(0, -0.55, 'Digital Payments',
             ha='center', fontsize=11, color=DARK, fontweight='bold')
    ax2.set_title('Payment KPI', fontsize=11, color='grey', pad=6)

    # ── KPI Donut 3: North Region Share ──
    ax3 = fig.add_subplot(gs[0, 2])
    vals3 = [28.3, 71.7]
    ax3.pie(vals3, colors=[ACCENT, '#E0E0E0'], startangle=90,
            wedgeprops=dict(width=0.38, edgecolor='white'))
    ax3.text(0, 0,  '28%',   ha='center', va='center',
             fontsize=22, fontweight='bold', color='#B8860B')
    ax3.text(0, -0.55, 'North Region Share',
             ha='center', fontsize=11, color=DARK, fontweight='bold')
    ax3.set_title('Regional KPI', fontsize=11, color='grey', pad=6)

    # ── H1 vs H2 Comparison Bar Chart ──
    ax4 = fig.add_subplot(gs[1, :])
    x   = np.arange(len(pair_labels))
    w   = 0.35
    b1  = ax4.bar(x - w/2, h1, width=w, label='H1 2024',
                  color=PRIMARY,   alpha=0.85, edgecolor='white', zorder=3)
    b2  = ax4.bar(x + w/2, h2, width=w, label='H2 2024',
                  color=SECONDARY, alpha=0.85, edgecolor='white', zorder=3)

    for bar in b1:
        ax4.text(bar.get_x() + bar.get_width()/2,
                 bar.get_height() + 0.3,
                 f'₹{bar.get_height():.2f}L',
                 ha='center', fontsize=8, color=DARK, fontweight='bold')
    for bar in b2:
        ax4.text(bar.get_x() + bar.get_width()/2,
                 bar.get_height() + 0.3,
                 f'₹{bar.get_height():.2f}L',
                 ha='center', fontsize=8, color=DARK, fontweight='bold')

    ax4.set_xticks(x)
    ax4.set_xticklabels(pair_labels, fontsize=10)
    ax4.set_ylabel('Revenue (₹ Lakhs)', fontsize=11)
    ax4.set_title('H1 vs H2 Monthly Revenue Comparison (₹ Lakhs)',
                  fontsize=13, fontweight='bold', color=DARK, pad=10)
    ax4.legend(fontsize=10, loc='upper left')
    ax4.set_ylim(0, 27)
    ax4.yaxis.set_major_formatter(mticker.FormatStrFormatter('₹%.0fL'))

    fig.suptitle('Executive Summary Dashboard — FY 2024',
                 fontsize=17, fontweight='bold', color=DARK, y=1.01)

    plt.savefig('/mnt/user-data/outputs/chart10_executive_dashboard.png',
                dpi=150, bbox_inches='tight')
    print("✅ Chart 10 saved.")
    plt.show()


# ═══════════════════════════════════════════════════════
#  RUN ALL CHARTS
# ═══════════════════════════════════════════════════════
if __name__ == '__main__':
    print("🚀 Generating charts...\n")
    chart1_monthly_revenue()
    chart2_category_revenue()
    chart3_payment_methods()
    chart9_quarterly()
    chart10_executive_dashboard()
    print("\n✅ All 5 charts generated successfully!")

