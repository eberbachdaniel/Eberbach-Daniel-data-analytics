
# Data Analytics Portfolio – Daniel Eberbach

Welcome to my data analytics portfolio. Here, I showcase projects that demonstrate my skills in Excel, SQL, and Tableau. These projects highlight my ability to analyze datasets, extract insights, and present findings in a clear and professional way.

---

# Gordon's Archery Performance Analysis

## Project Overview
This project explores Gordon's archery scores using descriptive analysis in Excel. The goal was to evaluate general performance patterns over multiple shooting sessions and comment on scoring consistency based on observed totals and round outcomes.

## File: `Gordon's Archery Analyzed.xlsx`
This Excel workbook includes:
- Organized session-by-session score tracking
- Observations based on high and low scores within each session
- Total scores used to identify solid rounds (e.g., above 50 points)
- Notes regarding mix of strong (e.g., 10s) and weak (e.g., 6s) shots

## Tools Used
- Microsoft Excel: Data entry, totals, manual trend observations, and basic charts

## Key Questions Answered
- Did the scores generally stay within a consistent range?
- Were there sessions with mixed performance (e.g., high scores paired with low)?
- Were certain rounds clearly stronger based on total points?

## Visuals Included
- Score breakdown per session
- Basic line or bar charts showing score totals
- Manually noted observations on scoring patterns

---

# Superstore Sales & Shipping Delay Analysis

## Project Overview
This project analyzes the Superstore sales dataset to uncover performance trends and inefficiencies in shipping across U.S. states, categories, and shipping methods. Using SQL queries and Tableau visualizations, I identified key delay patterns and regional disparities.

## Tools Used
- BigQuery (SQL): Grouping, filtering, subqueries, JOINs
- Tableau: Dashboards and visualizations

## Key Questions Answered
- Which state/category/ship mode combinations are responsible for the longest delays?
- How does each state’s shipping delay compare to its regional average?
- Do higher sales volumes correlate with longer delays?

## Summary of Insights
- Many state/category/ship mode combinations had average shipping delays over 50 days.
- Standard Class shipping had the highest average delay across all modes.
- Several states performed worse than their regional average in delay metrics.

## SQL Queries Used

### 1. Filtered and Grouped Delay by State
```sql
SELECT 
  `state`,
  `category`,
  `Region`,
  `Ship Mode`,
  SUM(`sales`) as Total_sales,
  AVG(Date_diff(`ship date`,`order date`, DAY)) as avg_delay
FROM
  eberbach-portfolio.superstore_sales.Store_sales
WHERE 
  Date_diff(`ship date`,`order date`, DAY) >= 0
  AND `postal Code` IS NOT NULL
GROUP BY 
  `category`,
  `Region`,
  `state`,
  `Ship Mode`
HAVING 
  avg_delay >= 50
ORDER BY
  avg_delay DESC, 
  Total_sales DESC;
```

### 2. State Delay vs. Regional Average Delay
```sql
SELECT
  a.state,
  a.Region,
  AVG(DATE_DIFF(a.`ship date`, a.`order date`, DAY)) AS state_delay,
  regional_delays.avg_region_delay
FROM 
  `eberbach-portfolio.superstore_sales.Store_sales` a
JOIN (
  SELECT
    Region,
    AVG(DATE_DIFF(`ship date`, `order date`, DAY)) AS avg_region_delay
  FROM 
    `eberbach-portfolio.superstore_sales.Store_sales`
  WHERE 
    DATE_DIFF(`ship date`, `order date`, DAY) >= 0
  GROUP BY Region
) AS regional_delays
  ON a.Region = regional_delays.Region
GROUP BY 
  a.state, a.Region, regional_delays.avg_region_delay
HAVING 
  state_delay > avg_region_delay
ORDER BY 
  state_delay DESC;
```

## Tableau Dashboard
The Tableau file `Superstore Sales & Shipping Analysis.twb` includes:
- Bar chart: Average Shipping Delay by Ship Mode
- Treemap or bubble chart: Total Sales by Category
- State map: Average Delay by State
- Filters for Region, Category, Ship Mode, and Manager

## Files Included
- `Gordon's Archery Analyzed.xlsx`
- `store_sales_queries.pdf`
- `Superstore Sales & Shipping Analysis.twb`

---

## About Me
I'm Daniel Eberbach, an aspiring data analyst with a background in business management and technical skills in Excel, SQL, and Tableau. My goal is to use data to solve real problems and deliver clear insights.

Email: eberbcahdniel@gmail.com  
LinkedIn: https://www.linkedin.com/in/DanielEberbach

Thank you for reviewing my portfolio!
