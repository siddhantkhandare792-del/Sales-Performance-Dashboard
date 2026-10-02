# Sales-Performance-Dashboard# 📊 Sales Performance Dashboard (Power BI)

An interactive, single-page Power BI dashboard that analyses global sales performance across **regions, countries, product categories, sales channels and order priorities**. It turns raw order-level data into clear KPIs and visuals so that managers can quickly see where revenue and profit come from, how performance changes over time, and how efficiently orders are fulfilled.

![Dashboard Preview](images/dashboard_preview.png)
> *Replace the image above with a screenshot of your dashboard (File → Export → or use Snipping Tool) saved as `images/dashboard_preview.png`.*

---

## 📑 Table of Contents
1. [Project Overview](#-project-overview)
2. [Business Problem](#-business-problem)
3. [Dataset](#-dataset)
4. [Tools & Technologies](#-tools--technologies)
5. [Data Preparation (Power Query)](#-data-preparation-power-query)
6. [Data Model](#-data-model)
7. [DAX Measures](#-dax-measures)
8. [Dashboard Walkthrough](#-dashboard-walkthrough)
9. [Design & Theme](#-design--theme)
10. [Key Insights](#-key-insights)
11. [How to Use This Project](#-how-to-use-this-project)
12. [Publishing to Power BI Service](#-publishing-to-power-bi-service)
13. [Repository Structure](#-repository-structure)
14. [Skills Demonstrated](#-skills-demonstrated)
15. [Future Improvements](#-future-improvements)
16. [Author](#-author)

---

## 🔎 Project Overview

| Item | Detail |
|---|---|
| **Project name** | Sales Performance Dashboard |
| **Tool** | Microsoft Power BI Desktop |
| **Dataset** | Sample Sales Records |
| **Time period** | February 2010 – May 2017 |
| **Scope** | 100 orders · 76 countries · 7 regions |
| **Pages** | 1 (single-page executive dashboard, 1280 × 896) |
| **File** | `Sales_Performance_Dashboard.pbix` |

---

## 🎯 Business Problem

Sales leadership needs a single view that answers:

- How much **revenue and profit** has the business generated, and at what **margin**?
- How has performance **trended year over year**?
- Which **regions** and **countries** contribute the most?
- Which **product categories (Item Types)** drive revenue and profit?
- How is business split between **Online and Offline** sales channels?
- How does **order priority** relate to revenue?
- How quickly are orders **shipped** on average?

---

## 🗂 Dataset

**Source:** Sample Sales Records (public sample dataset)

| Column | Description |
|---|---|
| Region | Geographic region of the sale (7 regions) |
| Country | Country of the sale (76 countries) |
| Item Type | Product category (e.g., Cosmetics, Household, Office Supplies) |
| Sales Channel | Online / Offline |
| Order Priority | C = Critical, H = High, M = Medium, L = Low |
| Order Date | Date the order was placed |
| Order ID | Unique order identifier |
| Ship Date | Date the order was shipped |
| Units Sold | Quantity sold |
| Unit Price | Selling price per unit |
| Unit Cost | Cost per unit |
| Total Revenue | Units Sold × Unit Price |
| Total Cost | Units Sold × Unit Cost |
| Total Profit | Total Revenue − Total Cost |

---

## 🛠 Tools & Technologies

- **Power BI Desktop** – data modelling and report design
- **Power Query (M)** – data cleaning and transformation
- **DAX** – calculated columns and measures
- **Power BI Service** – publishing and sharing
- **Custom JSON theme** – consistent colour palette and styling

---

## 🧹 Data Preparation (Power Query)

Steps applied to the raw data:

1. Imported the source file and promoted headers.
2. Set correct data types (dates, whole numbers, decimals, text).
3. Checked for and removed blanks and duplicates.
4. Created helper columns used in the visuals:
   - **Order Year** – year extracted from Order Date
   - **Region Short** – shortened region names for cleaner axis labels
   - **Priority Label** – full text for priority codes (C → Critical, H → High, M → Medium, L → Low)
   - **Ship Days** – number of days between Order Date and Ship Date

---

## 🧩 Data Model

The model contains three tables:

| Table | Type | Purpose |
|---|---|---|
| **Sales** | Fact table | Order-level transactions |
| **Date** | Date dimension | Calendar table for time-based analysis |
| **_Measures** | Measure table | Holds all DAX measures in one place for easy navigation |

**Relationship:** `Date[Date]` → `Sales[Order Date]` (one-to-many, single direction)

![Data Model](images/data_model.png)
> *Optional: add a screenshot of the Model view as `images/data_model.png`.*

---

## 🧮 DAX Measures

All measures are stored in the dedicated **_Measures** table.

> *The formulas below show the logic of each measure. Copy the exact DAX from your .pbix (Modeling view → select measure) if yours differs.*

```DAX
Total Revenue = SUM ( Sales[Total Revenue] )

Total Profit = SUM ( Sales[Total Profit] )

Profit Margin % = DIVIDE ( [Total Profit], [Total Revenue] )

Units Sold = SUM ( Sales[Units Sold] )

Orders = DISTINCTCOUNT ( Sales[Order ID] )

Avg Ship Days = AVERAGE ( Sales[Ship Days] )

Total Revenue (M) = [Total Revenue] / 1000000

Profit (M) = [Total Profit] / 1000000

Channel % =
DIVIDE (
    [Orders],
    CALCULATE ( [Orders], ALL ( Sales[Sales Channel] ) )
)

Dot = "●"   -- colour indicator used in the channel legend table
```

| Measure | What it shows |
|---|---|
| Total Revenue | Total sales value |
| Total Profit | Revenue minus cost |
| Profit Margin % | Profit as a share of revenue |
| Units Sold | Total quantity sold |
| Orders | Number of unique orders |
| Avg Ship Days | Average days from order to shipment |
| Total Revenue (M) / Profit (M) | Values in millions for compact chart labels |
| Channel % | Each channel's share of total orders |
| Dot | Coloured marker used as a custom legend |

---

## 📈 Dashboard Walkthrough

### 1. Header
Title **"Sales Performance Dashboard"** with a subtitle summarising the dataset scope (period, orders, countries, regions) on a custom background.

### 2. KPI Cards
Six headline cards across the top:
**Total Revenue · Total Profit · Profit Margin % · Units Sold · Orders · Avg Ship Days**

### 3. Revenue, Profit & Margin by Year
*Line and clustered column combo chart*
Columns show Revenue (M) and Profit (M) per **Order Year**; the line shows **Profit Margin %** on a secondary axis to compare growth against profitability.

### 4. Revenue & Profit by Region
*Clustered bar chart, ranked by revenue*
Compares Revenue (M) and Profit (M) across the 7 regions.

### 5. Revenue by Item Type
*Clustered bar chart*
Revenue (M) by product category. Darker bars highlight above-median profit, and margin is shown in brackets in the labels.

### 6. Top 10 Countries
*Clustered bar chart with a Top N filter*
The ten highest-revenue countries.

### 7. Sales Channel
*Donut chart + order count card + legend table*
Order split between Online and Offline, with a custom table showing each channel's colour dot and **Channel %**.

### 8. By Priority
*Clustered column chart*
Revenue (M) by order priority (Critical, High, Medium, Low).

### Interactivity
- **Cross-filtering:** clicking any bar, column or donut slice filters every other visual on the page.
- **Tooltips:** hover over any visual for exact values.
- **Filter pane:** available for additional slicing in the Power BI Service.

---

## 🎨 Design & Theme

- **Custom theme:** `Sales Performance Dashboard` (JSON)
- **Primary palette:**

| Colour | Hex | Used for |
|---|---|---|
| Navy | `#1E2A47` | Header, primary series |
| Teal | `#1B998B` | Profit / secondary series |
| Orange | `#F28C38` | Accents and highlights |
| Steel Blue | `#3A6EA5` | Additional series |
| Grey | `#6B7280` | Subtitles and labels |

- Custom background image for a clean, card-based layout
- Consistent titles and subtitles explaining what each visual shows
- Values shown in millions (M) to keep labels readable

---

## 💡 Key Insights

> *Fill these in from your own dashboard so the numbers are accurate.*

- Total revenue of **$___M** with a total profit of **$___M** (margin **___%**).
- **___** was the strongest year for revenue, while **___** had the highest margin.
- **___** is the top region, contributing **___%** of revenue.
- **___** is the top product category by revenue; **___** has the highest margin.
- **___** is the leading country by revenue.
- Orders are split **___% Online / ___% Offline**.
- On average, orders are shipped in **___ days**.

---

## ▶️ How to Use This Project

1. Install **[Power BI Desktop](https://powerbi.microsoft.com/desktop/)** (free, Windows).
2. Clone or download this repository:
   ```bash
   git clone https://github.com/<your-username>/Sales-Performance-Dashboard.git
   ```
3. Open `Sales_Performance_Dashboard.pbix` in Power BI Desktop.
4. If prompted, update the data source path: **Home → Transform data → Data source settings → Change Source**, and point it to the file in the `data/` folder.
5. Click **Refresh** and explore the dashboard.

---

## 🌐 Publishing to Power BI Service

1. Open the `.pbix` file in Power BI Desktop.
2. Click **Home → Publish**.
3. Sign in with your Power BI (work or school) account.
4. Choose a workspace (e.g., **My workspace**) and click **Select**.
5. Once published, click **Open in Power BI** to view it online.
6. To share:
   - **Share** button → enter email addresses (requires Power BI Pro or Premium), **or**
   - **File → Embed report → Publish to web (public)** to get a public link and embed code for a portfolio or GitHub.
     ⚠️ *Only use Publish to web for non-confidential data — anyone with the link can view it.*

🔗 **Live dashboard:** [View on Power BI](https://app.powerbi.com/your-public-link-here)

---

## 📁 Repository Structure

```
Sales-Performance-Dashboard/
│
├── Sales_Performance_Dashboard.pbix   # Power BI report file
├── README.md                          # Project documentation
├── data/
│   └── Sample_Sales_Records.csv        # Source dataset
└── images/
    ├── dashboard_preview.png          # Dashboard screenshot
    └── data_model.png                 # Model view screenshot
```

---

## 🧠 Skills Demonstrated

- Data cleaning and transformation with **Power Query**
- Data modelling with a **fact table, date table and measure table**
- Writing **DAX** measures (aggregations, ratios, DIVIDE, CALCULATE, ALL)
- **Top N** filtering and combo charts with dual axes
- Conditional formatting and custom legends
- Dashboard layout, theming and **storytelling with data**
- Publishing and sharing via **Power BI Service**

---

## 🚀 Future Improvements

- Add **slicers** for Year, Region and Sales Channel
- Add **Year-over-Year growth** and **YTD** measures using time intelligence
- Add a **drill-through page** for country-level detail
- Add a **map visual** for geographic distribution
- Connect to a larger dataset and set up **scheduled refresh**

---

## 👤 Author

**Siddhant Khandare**
Data Analyst | Power BI · SQL · Python

- 📧 Email: siddhantkhandare792@gmail.com
- 💼 LinkedIn: www.linkedin.com/in/siddhant-khandare-220b84114
- 📱 Phone: +91 7756961603

⭐ *If you found this project useful, please give it a star!*
