# Module 7: Advanced Analytics

> [!quote] Business Context
> **From**: Business Intelligence Team  
> **To**: Analytics Team  
> **Need**: We need sophisticated analytics that go beyond simple sums and counts. Rankings, moving averages, percentiles - these help us understand relative performance.

---

## Problem 7.1: Monthly Product Rankings Within Category

> [!question] The Business Question
> **"Which products are winning within their category each month?"**
> 
> Rankings change over time - we want to track product performance evolution.

**📋 What You Need to Deliver:**

For each month, rank products WITHIN their category:

- Month
- Category
- Product Name
- Monthly Revenue
- Rank Within Category That Month (1 = best in category)

> [!tip] Key Concept
> This isn't "overall top 10 products" - it's "top product IN EACH CATEGORY" for each month.
> 
> Think: How do you rank products separately within each category group?

---

## Problem 7.2: Moving Average Revenue

> [!question] The Business Question
> **"What's the underlying trend without daily volatility?"**
> 
> Daily sales jump around - moving averages smooth this out to see the real trend.

**📋 What You Need to Deliver:**

For each day:

- Date
- Daily Revenue
- 7-Day Moving Average
- 30-Day Moving Average

> [!note] What is a Moving Average?
> A 7-day moving average for June 7th = average of June 1-7
> A 7-day moving average for June 8th = average of June 2-8
> 
> It "moves" forward one day at a time, always looking at the last N days.

> [!example] Why This Matters
> ```
> Date     | Daily    | 7-Day Avg
> June 1   | $50,000  | $50,000 (just 1 day so far)
> June 2   | $45,000  | $47,500 (avg of 2 days)
> June 3   | $55,000  | $50,000
> June 8   | $52,000  | $51,000 (avg of 7 days)
> June 9   | $48,000  | $50,500 (avg of last 7 days)
> ```

---

## Problem 7.3: Product Performance Percentiles

> [!question] The Business Question
> **"Where does each product fall in the overall distribution?"**
> 
> Is a product in the top 10%? Top 25%? Bottom half?

**📋 What You Need to Deliver:**

For each product:

- Product Name
- Total Revenue
- Percentile (0-100, where 100 = highest revenue)
- Quartile (Q1, Q2, Q3, or Q4)

> [!note] Understanding Percentiles
> - 90th percentile = better than 90% of products
> - 50th percentile = median (middle)
> - 10th percentile = only better than 10% of products
> 
> Quartiles divide into 4 equal groups:
> - Q4 = Top 25%
> - Q3 = 25-50%
> - Q2 = 50-75%
> - Q1 = Bottom 25%

---

## Problem 7.4: Customer Revenue Contribution

> [!question] The Business Question
> **"What percentage of revenue comes from our top customers?"**
> 
> This answers: Do we depend heavily on a few big customers? (The 80/20 rule)

**📋 What You Need to Deliver:**

For each customer (sorted by revenue):

- Customer Name
- Customer Revenue
- Cumulative Revenue (running total)
- Cumulative Percentage of Total Revenue
- Customer Rank

Then answer: What % of revenue comes from top 20% of customers?

> [!note] The 80/20 Rule (Pareto Principle)
> Often, 80% of revenue comes from 20% of customers. This is important because:
> - Losing a top customer is devastating
> - We should treat top customers very well
> - We can identify who those critical customers are

> [!example] What You'll See
> ```
> Customer   | Revenue  | Cumulative | Cum % | Rank
> Alice Corp | $500,000 | $500,000   | 2.5%  | 1
> Bob Ltd    | $450,000 | $950,000   | 4.8%  | 2
> Carol Inc  | $400,000 | $1,350,000 | 6.8%  | 3
> ...
> [After 20% of customers]        | 82.3% | ...
> ```

---

## 💡 Module Summary

In this module, you learned to:
- Use window functions (RANK, ROW_NUMBER, PARTITION BY)
- Calculate moving averages with window frames
- Compute percentiles and quartiles
- Calculate cumulative sums and percentages
- Perform advanced ranking within groups
- Apply the Pareto principle to business data

---

← [[Module-06-Query-Organization|← Previous: Module 6]] | [[00-Index|Home]] | [[Module-08-Multi-Dimensional-Analysis|Next: Module 8 →]]