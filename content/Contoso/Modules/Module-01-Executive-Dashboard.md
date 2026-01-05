# Module 1: Executive Dashboard Questions

> [!quote] Business Context
> **From**: CEO and Executive Team  
> **To**: Data Analytics Team  
> **Need**: We need a weekly snapshot of business performance to make strategic decisions. Our investors and board want to see if we're healthy and growing.

---

## Problem 1.1: Executive KPI Dashboard

> [!question] The Business Question
> **"Is our business performing well this quarter?"**
> 
> The executives need one summary view showing our overall health.

**📋 What You Need to Deliver:**

Return **ONE ROW** with these metrics:

- Total Revenue (how much money we made from sales)
- Total Cost (how much we spent to get/make the products)
- Gross Profit (Revenue minus Cost)
- Profit Margin % (what percentage is profit)
- Total Number of Customers (unique customers who bought)
- Total Number of Orders (distinct orders placed)
- Average Order Value (average $ amount per order)
- Total Units Sold (how many individual items sold)

> [!tip] Getting Started
> - Start with the `sales` table - it has everything you need
> - Revenue = quantity × netprice
> - Cost = quantity × unitcost
> - Remember: Watch out for dividing by zero when calculating percentages
> - Use ROUND() to make numbers readable (2 decimal places)

> [!example] What Good Output Looks Like
> ```
> total_revenue | total_cost | gross_profit | profit_margin | customers | orders | avg_order_value | units_sold
> 523,456,789   | 245,678,901| 277,777,888  | 53.08%        | 1,234,567 | 456,789| 1,145.67        | 12,345,678
> ```

---

## Problem 1.2: Year-over-Year Performance

> [!question] The Business Question
> **"Are we growing compared to last year?"**
> 
> The board wants to see if we're improving year-over-year.

**📋 What You Need to Deliver:**

Return **ONE ROW** comparing 2016 vs 2017:

- Total Revenue for 2016
- Total Revenue for 2017
- Revenue Growth Amount (2017 - 2016)
- Revenue Growth Percentage
- Order Count for 2016
- Order Count for 2017
- Order Growth Percentage

> [!tip] Hints to Get You Started
> - You'll need to filter by year from the `orderdate` column
> - Think about how to get two different years in the same row
> - CASE statements can help put different years in different columns
> - Or you could use WITH clauses to calculate each year separately

---

## Problem 1.3: Current Month Performance

> [!question] The Business Question
> **"How are we tracking this month against our target?"**
> 
> It's mid-month and management wants to know if we'll hit our goals.

**📋 What You Need to Deliver:**

For June 2016 (treat this as "current" month), show:

- Current Month Revenue
- Current Month Order Count
- Average Daily Revenue (total revenue ÷ number of days)
- Number of Selling Days (how many days had sales)
- Target Monthly Revenue (assume target is $50,000,000)
- Percentage of Target Achieved

> [!tip] Approach This Problem By
> - Filter dates to June 2016 only
> - Count distinct days to get selling days
> - Calculate how you're doing vs the $50M target
> - Think: Is 60% of target good if we're only 50% through the month?

> [!warning] Common Mistakes
> - Forgetting to filter to just June 2016
> - Using total rows instead of distinct days
> - Not rounding percentages to be readable

---

## 💡 Module Summary

In this module, you learned to:
- Calculate aggregate metrics (SUM, COUNT, AVG)
- Work with financial calculations (revenue, cost, profit)
- Handle date filtering and year comparisons
- Calculate percentages and ratios
- Present single-row summary reports

---

← [[00-Index|Home]] | [[Module-02-Product-Analytics|Next: Module 2 →]]