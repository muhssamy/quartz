# Module 3: Customer Analytics

> [!quote] Business Context
> **From**: Marketing Team  
> **To**: Analytics Team  
> **Need**: We need to understand our customer base - who are our best customers? How should we segment them for targeted campaigns? Which channels work best?

---

## Problem 3.1: Customer Lifetime Value (CLV)

> [!question] The Business Question
> **"Who are our most valuable customers and how can we keep them?"**
> 
> We want to identify VIP customers for special treatment and loyalty programs.

**📋 What You Need to Deliver:**

Show the **top 50 customers** by total spend with:

- Customer Name (First + Last)
- Customer Country
- Total Revenue Generated (their lifetime value)
- Total Orders Placed
- Average Order Value
- First Purchase Date
- Last Purchase Date
- Customer Tenure (days as a customer)

> [!tip] Building This Query
> - Join customers to sales
> - Group by customer
> - Calculate aggregations per customer
> - Sort by total spend
> - Limit to top 50
> - Combine first and last name into one field

> [!example] Why This Matters
> A customer who spent $50,000 over 5 years and 100 orders is more valuable than one who spent $50,000 in 1 order - they're loyal and engaged!

---

## Problem 3.2: RFM Segmentation

> [!question] The Business Question
> **"How should we segment customers for targeted marketing campaigns?"**
> 
> Different customer segments need different marketing approaches.

**📋 What You Need to Deliver:**

Segment customers by three dimensions:

**Recency** - When did they last buy?
- Recent: < 30 days ago
- Moderate: 30-90 days ago
- Lapsed: 90-180 days ago
- Lost: > 180 days ago

**Frequency** - How often do they buy?
- One-time: 1 order
- Occasional: 2-5 orders
- Regular: 6-10 orders
- Loyal: >10 orders

**Monetary** - How much do they spend?
- Low: < $1,000
- Medium: $1,000 - $5,000
- High: $5,000 - $10,000
- VIP: > $10,000

Show: How many customers in each combination?

> [!note] Marketing Strategies by Segment
> - **Recent + Frequent + High Spend**: VIP loyalty program
> - **Lapsed + Frequent + High Spend**: Win-back campaign urgently!
> - **Recent + Infrequent + Low Spend**: Upsell campaign
> - **Lost + Infrequent + Low Spend**: Probably not worth pursuing

> [!tip] How to Approach
> - Calculate recency (days since last purchase)
> - Calculate frequency (count of orders)
> - Calculate monetary (sum of spend)
> - Use CASE statements to bucket into categories
> - Group by all three segments
> - Count customers in each bucket

---

## Problem 3.3: Customer Acquisition by Channel

> [!question] The Business Question
> **"Should we invest more in online or in-store customer acquisition?"**
> 
> We need to compare which channel brings better customers.

**📋 What You Need to Deliver:**

Compare two channels:

- Online Channel (customers who buy online)
- Store Channel (customers who buy in-store)

For each channel show:

- Number of Customers
- Total Revenue
- Average Customer Lifetime Value
- Average Orders Per Customer
- Revenue Per Customer

> [!tip] Think About
> - How do you identify online vs store customers from the data?
> - Should a customer who uses both channels count in both?
> - Calculate customer-level metrics first, then aggregate

> [!example] Business Insight
> If online customers have higher lifetime value but there are fewer of them, should we invest in getting more online customers? Or improving their experience?

---

## Problem 3.4: Geographic Customer Distribution

> [!question] The Business Question
> **"Where should we open new stores?"**
> 
> We need to understand where our customers are located to guide expansion.

**📋 What You Need to Deliver:**

Show customer distribution by **country** and **state**:

- Country
- State
- Number of Customers
- Total Revenue
- Average Revenue Per Customer
- Percentage of Total Revenue

Sort by revenue within each country.

> [!tip] Multi-Level Grouping
> - Group by both country AND state
> - Calculate aggregations for each combination
> - You'll need to calculate "percentage of total" somehow
> - Think about which countries/states are underserved

> [!warning] Challenge
> How do you calculate "percentage of total revenue" for each state? You need the grand total somewhere!

---

## 💡 Module Summary

In this module, you learned to:
- Calculate customer lifetime metrics
- Perform multi-dimensional segmentation (RFM)
- Compare customer segments
- Work with geographic hierarchies
- Calculate percentages of totals
- Translate data into marketing strategies

---

← [[Module-02-Product-Analytics|← Previous: Module 2]] | [[00-Index|Home]] | [[Module-04-Time-Series-Analysis|Next: Module 4 →]]