# Module 6: Query Organization

> [!quote] Business Context
> **From**: Analytics Team Lead  
> **To**: Junior Analysts  
> **Need**: As queries get complex, we need to organize them clearly. Complex problems should be broken into steps. Code should be readable!

---

## Problem 6.1: Above-Average Products

> [!question] The Business Question
> **"Which products are performing better than typical?"**
> 
> We want to identify over-performers to understand what makes them successful.

**📋 What You Need to Deliver:**

Find products where revenue is ABOVE the average product revenue:

- Product Name
- Product Revenue
- Average Product Revenue (for comparison)
- How much above average ($ and %)

> [!tip] The Subquery Challenge
> - First, what IS the average product revenue?
> - Then, which products exceed it?
> - You need to calculate the average, then compare each product to it
> - Could use subqueries or temp calculations

---

## Problem 6.2: Top 3 Customers Per Country

> [!question] The Business Question
> **"Who are our top customers in each country?"**
> 
> We want to recognize and reward top customers in each market.

**📋 What You Need to Deliver:**

For each country, show the TOP 3 customers by revenue:

- Country
- Customer Name
- Revenue
- Rank (1, 2, or 3 within that country)

> [!tip] Top-N Per Group Pattern
> - This is harder than just "top 3 overall"
> - You need top 3 within EACH country
> - Think: How do you rank customers within their country group?

> [!example] Expected Result
> ```
> Country   | Customer Name  | Revenue    | Rank
> Australia | John Smith     | $250,000   | 1
> Australia | Jane Doe       | $220,000   | 2
> Australia | Bob Jones      | $215,000   | 3
> France    | Marie Curie    | $180,000   | 1
> France    | Pierre Blanc   | $175,000   | 2
> ...
> ```

---

## Problem 6.3: Strong Growth Months

> [!question] The Business Question
> **"Which months showed exceptional growth?"**
> 
> We want to study months with >10% growth to understand what drove success.

**📋 What You Need to Deliver:**

Show only months where revenue grew by MORE than 10% vs previous month:

- Year
- Month
- Revenue
- Previous Month Revenue
- Growth Percentage

> [!tip] Breaking It Into Steps
> Instead of one giant query, break it down:
> 
> 1. Calculate monthly revenue
> 2. Calculate previous month's revenue for comparison
> 3. Calculate growth percentage
> 4. Filter to only >10% growth
> 
> Use WITH clauses (CTEs) to organize these steps!

---

## Problem 6.4: Customer Cohort Analysis

> [!question] The Business Question
> **"How well do we retain customers over time?"**
> 
> Group customers by when they first bought (cohort), then track their activity over following months.

**📋 What You Need to Deliver:**

For each cohort (acquisition month):

- Cohort Month (when these customers first bought)
- Cohort Size (number of customers)
- Revenue in Month 1, Month 2, Month 3, Month 6 after acquisition
- Retention Rate (% who bought again)

> [!note] What is a Cohort?
> A cohort is a group of customers who started in the same time period. For example:
> - "January 2016 cohort" = customers whose first purchase was January 2016
> - Track them over time: How many bought again in Feb? March? etc.

> [!warning] This is Complex!
> This is one of the harder problems. Break it into steps:
> 1. Find each customer's first purchase month (their cohort)
> 2. Find their purchases in subsequent months
> 3. Calculate retention for each cohort

---

## 💡 Module Summary

In this module, you learned to:
- Use subqueries to calculate benchmarks
- Organize complex queries with CTEs (WITH clauses)
- Perform ranked/top-N per group queries
- Break complex problems into logical steps
- Write maintainable, readable SQL code
- Perform cohort analysis

---

← [[Module-05-Advanced-Relationships|← Previous: Module 5]] | [[00-Index|Home]] | [[Module-07-Advanced-Analytics|Next: Module 7 →]]