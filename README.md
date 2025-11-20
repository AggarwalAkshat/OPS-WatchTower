# The OPS WatchTower - Ontaio Crime Insights

A 6-page Microsoft Fabric + Power BI crime analytics dashboard built for an internal hackathon at the Ontario Public Service (OPS) by team **The OPS WatchTower**.

The report ingests public crime statistics and justice system data (StatsCan) into Fabric, models it in a semantic model, and surfaces interactive insights for decision-makers about **crime trends, regional hotspots, justice system performance, and offence-level patterns** across Ontario.

---

## 📁 Repository contents

- report/ – Power BI report (.pbix), PDF export, and PowerPoint export
- screenshots/ – per-page screenshots used in documentation and portfolio
- design/ – team logo assets for The OPS WatchTower
- data/ – (optional) public sample CSVs / links to StatsCan datasets
- docs/measures.md – key DAX measures and modelling notes

---

## 🎯 Goals & impact

- The OPS WatchTower report was designed to help decision-makers:
  - Understand long-term crime trends and forecast future incidents
  - Identify regional hotspots where rates are consistently higher
  - See whether courts and corrections are keeping up with incident volumes
  - Drill into specific offence types to support targeted policy and resource decisions
  - Built during an internal OPS hackathon, the project demonstrates how Microsoft Fabric + Power BI can turn raw public data into an interactive, narrative-driven analytical experience.

---

## 🧑‍💻 Team – The OPS WatchTower

- Team responsibilities included:
  - Data ingestion & cleaning in Fabric dataflows and Lakehouse tables
  - Semantic model design, relationships, and DAX measure creation
  - Report layout, theming, UX design, and visual storytelling
  - Hackathon narrative and demo (including a courtroom-themed intro to the problem)

---

## Logo:
The OPS WatchTower – a neon watchtower scanning across a circular grid, representing guardianship over Ontario’s data landscape and proactive monitoring of crime and justice trends.

---

## 🏗️ Tech stack

- **Microsoft Fabric**
  - Dataflows for ingesting StatsCan CSVs
  - Lakehouse tables for curated datasets
  - Semantic model with relationships across crime, courts, corrections & population
- **Power BI**
  - 6-page report with slicers, heatmaps, forecasts, and drill-downs
  - DAX measures for derived metrics (e.g., uncleared incidents)
- **Data sources**
  - Public Statistics Canada crime and justice datasets for Ontario and CMAs

---

## 📊 Report pages

The final report contains six core pages:

1. ### Ontario Crime Overview  
   - KPI card showing **total incidents** across the selected year range  
   - **Year range** slider (2010–2024) to filter the entire report  
   - Line chart of **regional total incidents by year**  
   - Column chart of **crime rate per 100k by year**  
   - Detail table with yearly metrics (incidents, rate per 100k, persons charged, percent cleared)

2. ### Trends & 5-Year Forecast  
   - Line + forecast chart of **regional total incidents**, projecting **5 years into the future**  
   - Trend line for **crime rate per 100k** over time  
   - Comparative trends for **adult vs youth charged**  
   - Shared year slicer to zoom into specific time windows (e.g., pre/post policy changes)

3. ### Regional Crime & CMA Insights  
   - Year slicer to focus on a specific year or range  
   - Bar chart of **total CMA population** by region  
   - Table of CMAs with **population, total incidents, and crime rate per 100k**  
   - Combined incidents + rate per 100k trend for Ontario, highlighting how risk changes as population shifts

4. ### Heatmaps & Regional Hotspots  
   - Heatmap (matrix) of **crime rate per 100k by region and year**, using conditional formatting to highlight hotspots  
   - **Top 10 regions** bar chart for the selected year, ranked by crime rate per 100k  
   - Linked to the global year slicer so users can quickly compare different periods

5. ### Justice System Performance  
   - Top-row KPI cards:
     - **Incidents Cleared (%)**
     - **Total Court Cases**
     - **Corrections Admissions**
   - Line chart of **percent of incidents cleared by year**  
   - Combo chart comparing **court workload vs corrections admissions** over time (with secondary axis)  
   - Designed to show whether the justice system is keeping up with crime trends

6. ### Offence Type Explorer  
   - **Offence / Violation type slicer** (e.g., homicide, sexual assault, firearms, etc.)  
   - **Year range** slicer shared with other pages  
   - Line chart of **rate per 100k by year for the selected offence**  
   - Optional table of regions with their rate per 100k for that offence and period  
   - Lets analysts answer questions like: “How has the rate of \<offence\> changed over time, and where is it highest?”

*(A seventh page, “Geographic Hotspots”, was also explored using map visuals to show spatial patterns of crime rates.)*

---

## 🧠 Data modelling

The semantic model includes:

- `crime_annual_ontario` – provincial-level annual crime metrics  
- `crime_annual_region` – regional / CMA level metrics (incidents, rate per 100k, persons charged, percent cleared)  
- `population_annual_cma` – CMA population by year  
- `court_flow_annual` – total adult & youth court cases by year  
- `corrections_annual` – admissions and capacity metrics  
- `Calendar` – date dimension with Year/Date used for consistent slicing

Key relationships:

- `Calendar[Year]` → `crime_annual_ontario[year]`  
- `Calendar[Year]` → `crime_annual_region[year]`  
- `Calendar[Year]` → `population_annual_cma[year]`  
- `Calendar[Year]` → `court_flow_annual[year]`  
- `Calendar[Year]` → `corrections_annual[year]`  

Derived measures (examples):

```DAX
Uncleared Incidents =
    SUM(crime_annual_region[incident_count]) -
    SUM(crime_annual_region[cleared_incidents])

Crime Rate per 100k (Indexed) =
    DIVIDE( SUM(crime_annual_region[crime_rate_per_100k]),
            CALCULATE( SUM(crime_annual_region[crime_rate_per_100k]), MIN(Calendar[Year]) )
    )
