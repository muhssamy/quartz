# Module 5: Advanced Relationships

> [!quote] Business Context
> **From**: Operations Team  
> **To**: Analytics Team  
> **Need**: We need to integrate data across multiple systems and handle missing or incomplete data. Real-world data isn't perfect!

---

## Problem 5.1: Product Sales Coverage

> [!question] The Business Question
> **"Are all our products actually selling, or do we have dead inventory?"**
> 
> We need to know which products have NEVER sold.

**📋 What You Need to Deliver:**

Show **ALL PRODUCTS** (even ones with no sales):

- Product Name
- Category
- Total Units Sold (0 if never sold)
- Total Revenue (0 if never sold)
- Status: "Never Sold", "Low Sales" (<10 units), or "Active"

> [!warning] The Key Challenge
> Most queries only show products that HAVE sales. This query must include products with ZERO sales too!

> [!tip] Think About
> - What kind of join includes ALL products even without matches?
> - How do you handle NULL values from products without sales?
> - How do you show 0 instead of NULL?

> [!example] Why This Matters
> If you have 2,500 products but only 2,300 appear in sales, you have 200 products sitting in warehouse costing you money!

---

## Problem 5.2: Customer Purchase History Completeness

> [!question] The Business Question
> **"Do we have data quality issues between our systems?"**
> 
> Sometimes orders exist without sales records, or vice versa. This is a data integrity problem.

**📋 What You Need to Deliver:**

Find customers who have:

- Orders in the `orders` table BUT missing from `sales` table, OR
- Sales in the `sales` table BUT missing from `orders` table

Show:

- Customer Key
- Customer Name
- What's wrong (order without sales vs sales without order)

> [!tip] This is a Data Quality Check
> - In a perfect world, every order has corresponding sales rows
> - Look for mismatches
> - Think about what kind of joins find "orphaned" records

---

## Problem 5.3: Multi-Currency Revenue Consolidation

> [!question] The Business Question
> **"What's our true global revenue in USD?"**
> 
> We sell in multiple currencies - we need everything converted to USD for reporting.

**📋 What You Need to Deliver:**

Calculate total revenue in USD by:

- Joining sales to currency exchange rates
- Converting each sale to USD using the exchange rate from that day
- Show breakdown by original currency

For each currency show:

- Original Currency Code
- Order Count
- Revenue in Original Currency
- Revenue in USD

> [!tip] The Currency Challenge
> - Sales have a date and currency code
> - Exchange rates table has rates by date and currency
> - You need to join on BOTH date AND currency
> - Formula: Original Amount × Exchange Rate = USD Amount

> [!note] Why This Matters
> Without currency conversion, you can't add up sales from different countries. $100 USD ≠ $100 AUD!

---

## Problem 5.4: Store Performance with Demographics

> [!question] The Business Question
> **"Do local demographics predict store performance?"**
> 
> We want to know if stores in areas with more customers do better.

**📋 What You Need to Deliver:**

For each store:

- Store Location (Country, State)
- Number of Local Customers (customers from same state)
- Store Revenue
- Average Customer Age in that area

> [!tip] Complex Relationships
> - Join stores to sales (for revenue)
> - Join sales to customers (for demographics)
> - Match customers to stores by geography
> - This requires multiple joins!

---

## 💡 Module Summary

In this module, you learned to:
- Use LEFT/RIGHT/FULL OUTER joins to include non-matching rows
- Handle NULL values from outer joins
- Perform complex multi-table joins
- Join on multiple conditions (date AND currency)
- Identify data quality issues through join analysis
- Work with currency conversions

---

← [[Module-04-Time-Series-Analysis|← Previous: Module 4]] | [[00-Index|Home]] | [[Module-06-Query-Organization|Next: Module 6 →]]