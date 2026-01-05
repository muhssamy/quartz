# Module 4: Time Series Analysis

> [!quote] Business Context
> **From**: Finance Team  
> **To**: Analytics Team  
> **Need**: We need to understand trends over time, seasonal patterns, and whether we're growing. This helps with forecasting, budgeting, and spotting problems early.

---

## Problem 4.1: Monthly Revenue Trend

> [!question] The Business Question
> **"Are we growing month-over-month? What's the trend?"**
> 
> Finance needs to see if we're on an upward or downward trajectory.

**📋 What You Need to Deliver:**

Show monthly data:

- Year
- Month Name
- Total Revenue
- Total Orders
- Average Order Value
- Month-over-Month Revenue Change %

Sort chronologically (oldest first).

> [!tip] The Tricky Part
> How do you compare each month to the previous month in the same query? You need to "look back" one row somehow.

> [!example] What You're Looking For
> ```
> 2016 | January  | $4,500,000 | 15,234 | $295 | NULL (first month)
> 2016 | February | $4,950,000 | 16,890 | $293 | +10.0%
> 2016 | March    | $4,455,000 | 15,123 | $295 | -10.0%
> ```

---

## Problem 4.2: Day-of-Week Patterns

> [!question] The Business Question
> **"When should we schedule promotions and extra staff?"**
> 
> Operations needs to know which days are busiest.

**📋 What You Need to Deliver:**

Analyze sales by **day of week** (Monday through Sunday):

- Day Name
- Total Revenue
- Average Daily Revenue
- Order Count
- Percentage of Weekly Revenue

> [!tip] Extracting Day of Week
> - You'll need to extract day name from dates
> - Average daily revenue = total ÷ number of that day (e.g., count of all Mondays)
> - Percentage needs the weekly total

> [!example] Business Decisions From This
> If Saturday brings 25% of weekly revenue, we should:
> - Staff more people on Saturdays
> - Run promotions mid-week to boost slower days
> - Ensure inventory is stocked for Saturday

---

## Problem 4.3: Seasonal Product Demand

> [!question] The Business Question
> **"How should we plan inventory by season?"**
> 
> Different products sell better in different quarters - we need to stock accordingly.

**📋 What You Need to Deliver:**

For each **product category** and **quarter**:

- Category Name
- Quarter (Q1, Q2, Q3, Q4)
- Total Units Sold
- Percentage of Annual Volume for that category
- Rank (which quarter is best for this category)

> [!tip] Understanding Seasonality
> - Cameras might sell more in Q4 (holidays)
> - Audio equipment might be steady year-round
> - Group by both category and quarter
> - Calculate percentage of that category's annual total

> [!example] Business Application
> If Home Appliances sell 45% of annual volume in Q4, we should:
> - Build inventory in Q3
> - Plan promotions for Q4
> - Expect lower volume Q1-Q3

---

## Problem 4.4: Year-to-Date Running Totals

> [!question] The Business Question
> **"Are we on track to hit our annual target?"**
> 
> It's mid-year and we need to know if we'll reach our $100M annual goal.

**📋 What You Need to Deliver:**

For each day in 2016:

- Date
- Daily Revenue
- Year-to-Date Revenue (cumulative running total)
- Target YTD Revenue (assuming linear $100M goal)
- Percentage vs Target

> [!tip] Running Totals Concept
> A running total means: for each row, sum all previous rows plus current row.
> 
> Example:
> - Jan 1: $50,000 (YTD: $50,000)
> - Jan 2: $60,000 (YTD: $110,000)
> - Jan 3: $55,000 (YTD: $165,000)

> [!warning] Performance Note
> This query will be slow on large datasets - limit to one year!

---

## 💡 Module Summary

In this module, you learned to:
- Calculate month-over-month changes
- Extract and analyze day-of-week patterns
- Perform seasonal analysis by quarter
- Calculate running totals (cumulative sums)
- Compare actual vs target performance
- Work with date functions and time-based grouping

---

← [[Module-03-Customer-Analytics|← Previous: Module 3]] | [[00-Index|Home]] | [[Module-05-Advanced-Relationships|Next: Module 5 →]]