

# SA Road Safety Insights Dashboard (2020-2024)
An interactive Power BI dashboard analyzing South Australian road crash data (2020–2024), mapping incident locations and breaking down trends by severity, time, and crash types.

### 🚨 Looking at the Numbers

This project started because I wanted to see what the actual trends were for road crashes in South Australia. I’ve put together an interactive Power BI dashboard analyzing data from 2020 to 2024, aiming to spot spatial patterns, understand crash severity, and figure out the most common types of collisions.

It's one thing to see headlines about accidents, but putting all the data into one view helps show where the real problems are.

---

### 💡 Key Insights Captured

I focused on visualizing a few critical areas. If you open the dashboard, you’ll find:

*   **High-Level Totals:** The top row of KPIs immediately shows the scale: Total Accidents, Total Casualties, Fatalities, Serious Injuries, and Minor Injuries. The percentage comparisons are useful for quickly spotting year-over-year trends (e.g., seeing that fatalities are *down* substantially while minor injuries are *up* in this view).
*   **A Breakdown of Crash Types:** I categorized these with icons to make them clear: 'Out of Control Roll Over,' 'Objects & Pedestrian,' 'Intersection,' and 'Same-Direction' (this looks like one of the major contributors).
*   **The Temporal View (When is this happening?):**
    *   The large line chart tracks 'Casualties by Month and Year,' showing seasonality. It looks like there are distinct peaks and valleys that are worth digging into (e.g., May and October appear significant here).
    *   There is also a 'Day/Night' breakdown, which clearly shows that the vast majority of casualties (77%+) happen during daylight hours.
*   **Location, Location, Location:**
    *   The 'Casualties by Area' donut chart shows the split between Metropolitan (over 73%), Country, and City. This highlights that urban metro areas are where the highest number of incidents occur.
    *   I included a bubble map visualization (powered by Microsoft Bing), which lets you see exactly *where* these clusters of incidents are. It’s dense, suggesting a lot of localized problem areas.
*   **Road Type Specifics:** The bar chart gives a detailed look at casualties on 'Not Divided' roads, 'T-Junctions,' 'Cross Roads,' 'Freeways,' and more, helping narrow down what infrastructure may be a factor.

---

### 🔧 Tools and Approach

*   **Platform:** This was built entirely in **Power BI**. The layout was designed for high readability, prioritizing clear metrics and map interaction.
*   **Data Source:** This uses data covering road crash statistics in South Australia (2020–2024).

### 🚀 How to Use It / View It

This dashboard is meant to be explored, but I've made it accessible even if you don't use Power BI.

*   **Option 1: Dig into the interactive file (Recommended)**
    1. Clone this repository to get the `.pbix` file.
    2. Open the file in **Power BI Desktop**.
    3. Use the **'Select Year' slicer** in the top-right corner to focus on a specific year.
    4. **Click anything!** You can click on a specific crash type icon, a slice of the area donut, or a peak on the month chart, and the *entire* report will cross-filter.

*   **Option 2: Quick View (No software needed)**
    *   If you just want a quick look at the design and data highlights, check out the attached **screenshot images** included right here in the repository. They give you a high-res look at the pages without needing to install anything.
```
