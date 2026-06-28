# SA Road Safety Insights Dashboard (2020–2024)

An interactive Power BI dashboard analysing road crash data across South Australia from 2020 to 2024. It maps incident locations and breaks down trends by crash severity, time of day, road type, and collision category.

---

## Background

I built this because I wanted to go beyond the yearly road toll headlines and actually understand the patterns. When does the spike happen — and where? Are country roads really that much more deadly than metro ones? Does the number of crashes tell the same story as the number of fatalities? Turns out, often not.

The dashboard lets you cut across five years of data and find answers to those questions yourself.

---

## What the Data Shows

All percentages below are year-on-year comparisons relative to the previous year.

| Year | Accidents | Casualties | Fatalities | Serious Injuries | Minor Injuries |
|------|-----------|------------|------------|-----------------|----------------|
| 2020 | 11,534 | 4,877 | 146 | 846 | 3,885 |
| 2021 | 13,149 (+14.0%) | 5,368 (+10.1%) | 156 (+6.8%) | 961 (+13.6%) | 4,251 (+9.4%) |
| 2022 | 12,356 (−6.0%) | 4,324 (−19.4%) | 100 (−35.9%) | 755 (−21.4%) | 3,469 (−18.4%) |
| 2023 | 13,066 (+5.7%) | 4,585 (+6.0%) | 165 (+65.0%) | 917 (+21.5%) | 3,503 (+1.0%) |
| 2024 | 13,134 (+0.5%) | 4,738 (+3.3%) | 131 (−20.6%) | 924 (+0.8%) | 3,683 (+5.1%) |

A few things worth flagging in this data:

- **2022 stands out.** It recorded the lowest fatality count across the five years (100), with almost every metric dropping significantly. Yet by 2023, fatalities jumped 65% to 165 — the highest in the dataset — despite total accidents barely changing. The crash count and the death toll are telling very different stories.
- **2020 had the fewest accidents** but not the fewest casualties, which reflects how COVID-era roads may have changed driving behaviour more than crash frequency.
- **Metropolitan areas consistently account for ~73–77% of casualties** across all years, but country roads account for a disproportionate share of fatalities relative to traffic volume — something the map makes obvious when you compare dot density to road exposure.
- **Daylight hours account for roughly 77–80% of casualties** every year. This is partly a function of when most driving happens, but it's a useful counter to the assumption that night driving is the primary risk.
- **Not Divided roads dominate the road type breakdown** every single year, followed by Cross Roads and T-Junctions. Freeways are consistently near the bottom despite high traffic volumes.
- **Intersection and Same-Direction crashes** are the two largest crash type categories most years, each sitting above 1,400–1,800 incidents annually.

---

## Dashboard Features

Each year uses the same layout:

- **KPI cards** across the top showing totals and year-on-year change for Accidents, Casualties, Fatalities, Serious Injuries, and Minor Injuries
- **Crash Types panel** breaking down Out of Control/Rollover, Objects & Pedestrian, Intersection, and Same-Direction collisions
- **Casualties by Month line chart** showing seasonal patterns across the year
- **Casualties by Road Type bar chart** ranked by volume
- **Casualties by Area donut chart** split into Metropolitan, Country, and City
- **Day/Night donut chart** showing the daylight vs. nighttime breakdown
- **Bing Map** with incident bubbles showing geographic clustering of crashes across SA

Everything cross-filters. Click a crash type, a slice of the area chart, or a peak on the monthly line — the rest of the dashboard updates to match.

---

## Files in This Repo

```
├── SA_RoadSafety_Dashboard.pbit     # Power BI template file
├── screenshots/
│   ├── 2020_Dashboard.png
│   ├── 2021_Dashboard.png
│   ├── 2022_Dashboard.png
│   ├── 2023_Dashboard.png
│   └── 2024_Dashboard.png
└── README.md
```

The raw data is not included because it's publicly available and updated directly from the source (see below). The `.pbit` template will prompt you to point to your own copy of the data when you first open it.

---

## Data Source

**Source:** [data.sa.gov.au](https://data.sa.gov.au) — South Australia Government Open Data

The dataset covers reported road crashes across SA with fields for crash location, severity, road type, time of day, and collision type.

### Data Preparation (Power Query)

The raw data required a few transformations before it was usable in the dashboard:

- **Address consolidation:** Individual address fields were combined into a single full address string for geocoding in the Bing Map visual
- **Postcode correction:** Some postcodes in the source data were stored as 3-digit values — these were padded to the correct 4-digit format
- **Severity classification:** Casualties were categorised into Fatalities, Serious Injuries, and Minor Injuries based on the source fields
- **Year filtering:** Data was segmented by year to power the year slicer

All transformations were done in Power Query inside Power BI Desktop — no external scripts or preprocessing required.

---

## Data Modelling

The model uses a simple star schema with a one-to-many relationship between a dedicated Date table and the crash fact table, joined on the date field.

The Date table was built separately rather than relying on Power BI's auto date hierarchy. This gives full control over how time intelligence functions behave — particularly important for the year-on-year comparisons, where using the wrong date context can produce incorrect results when a year has incomplete data.

The relationship is one-to-many: one date in the Date table maps to potentially many crash records on that date. This is the standard setup for time intelligence in Power BI and is what makes the year slicer filter the entire report correctly.

### DAX Measures

The KPI cards across the top of each page aren't pulling raw columns — they're driven by measures. The core ones built for this dashboard:

- **Total Casualties** — sums casualties across all severity levels for the current filter context
- **Total Fatalities / Serious Injuries / Minor Injuries** — individual severity measures following the same pattern
- **Previous Year [Metric]** — uses `CALCULATE` with `SAMEPERIODLASTYEAR` to pull the equivalent figure from the prior year, respecting whatever the year slicer is set to
- **YoY % Change** — divides the difference between current and previous year by the previous year value, using `DIVIDE` to handle years with no prior data (2020 shows 0% rather than an error)
- **Crash Type grouping** — the four crash categories shown in the left panel are a classification measure that maps raw collision codes from the source data into the broader groups (Out of Control/Rollover, Objects & Pedestrian, Intersection, Same-Direction)

The YoY measure is what drives the red percentage figures under each KPI card. When the value is negative (a decrease), it still displays in red — which in road safety context means an improvement, not a warning. That's an intentional design choice: consistency in colour coding matters more than the usual traffic-light convention here.

---

## How to Use It

### Option 1 — Open the interactive file (recommended)

1. Download [Power BI Desktop](https://powerbi.microsoft.com/desktop/) (free)
2. Clone this repo or download the `.pbit` file
3. Open `SA_RoadSafety_Dashboard.pbit` — it will prompt you for the data source path
4. Point it to your downloaded copy of the SA crash data from data.sa.gov.au
5. Use the **Select Year** dropdown (top right) to switch between years
6. Click any visual element to cross-filter the entire dashboard

> **Tested on:** Power BI Desktop — recommend using a recent version (2024 or later) to avoid any rendering issues with the map visual.

### Option 2 — Screenshots only (no software needed)

All five years are included as screenshots in the `/screenshots` folder. They won't be interactive but give you a clear view of the layout and data.

---

## Limitations

- **Reported crashes only.** This dataset covers crashes reported to police. Minor incidents that weren't reported won't be in the numbers.
- **2020 context.** COVID-19 significantly reduced traffic volumes in 2020, which likely explains the lower accident count relative to other years. The data doesn't include vehicle kilometres travelled, so direct rate comparisons aren't possible.
- **Geocoding accuracy.** The Bing Map visual geocodes from address strings. A small number of records with incomplete or ambiguous addresses may not have plotted correctly.
- **Metro vs. country fatality rates.** The dashboard shows raw casualty counts by area, not rates adjusted for traffic exposure. Country roads look safer by volume but are not when you account for how few vehicles use them.
