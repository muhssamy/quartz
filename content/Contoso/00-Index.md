# SQL for Retail Analytics: Contoso Database

_Learn SQL Through Real Business Problems_

---

## 📚 Course Overview

> [!info] What You'll Learn
> This course teaches you SQL by solving real business problems using the **Contoso Retail Database**. You'll work with actual retail data including customers, products, orders, and sales transactions.

> [!tip] How to Use This Guide
> - Each module has business problems that get progressively harder
> - Read the business context to understand **why** we need the data
> - Focus on **what** the business needs, not just writing queries
> - Test your queries and verify results make sense
> - Ask yourself: "Does this answer the business question?"

---

## 🢠About Contoso Corporation

> [!example] Company Overview
> - **Industry**: Retail (consumer electronics and products)
> - **Headquarters**: Paris, France
> - **Operations**: Global presence across multiple countries
> - **Products**: 2,500+ items including Audio, Computers, Cameras, Home Appliances, Games, and more
> - **Customers**: 2+ million customers worldwide
> - **Sales Data**: 20+ million transaction records from 2016-2027
> - **Stores**: 200+ physical retail locations

> [!note] Available Data Tables
> - **`customer`** - Customer demographics, location, and personal information
> - **`product`** - Product catalog with categories, pricing, and specifications
> - **`store`** - Store locations, size, and operational details
> - **`date`** - Calendar information with fiscal periods and business days
> - **`sales`** - Individual transaction records (THE BIG TABLE!)
> - **`orders`** - Order header information
> - **`orderrows`** - Individual line items within orders
> - **`currencyexchange`** - Daily exchange rates for multi-currency transactions

> [!warning] Important Notes
> - The `sales` table has 20+ million rows - your queries might take time!
> - Always test with `LIMIT` first when exploring data
> - Join only the tables you need - unnecessary joins slow things down

---

## 📖 Course Modules

### Beginner Level (Weeks 1)

- [[Module-01-Executive-Dashboard|Module 1: Executive Dashboard Questions]]
  - Problem 1.1: Executive KPI Dashboard
  - Problem 1.2: Year-over-Year Performance
  - Problem 1.3: Current Month Performance

- [[Module-02-Product-Analytics|Module 2: Product Analytics]]
  - Problem 2.1: Category Performance Ranking
  - Problem 2.2: Top 10 Best-Selling Products
  - Problem 2.3: Product Portfolio Matrix
  - Problem 2.4: Slow-Moving Inventory Analysis

- [[Module-03-Customer-Analytics|Module 3: Customer Analytics]]
  - Problem 3.1: Customer Lifetime Value (CLV)
  - Problem 3.2: RFM Segmentation
  - Problem 3.3: Customer Acquisition by Channel
  - Problem 3.4: Geographic Customer Distribution

### Intermediate Level (Weeks 2)

- [[Module-04-Time-Series-Analysis|Module 4: Time Series Analysis]]
  - Problem 4.1: Monthly Revenue Trend
  - Problem 4.2: Day-of-Week Patterns
  - Problem 4.3: Seasonal Product Demand
  - Problem 4.4: Year-to-Date Running Totals

- [[Module-05-Advanced-Relationships|Module 5: Advanced Relationships]]
  - Problem 5.1: Product Sales Coverage
  - Problem 5.2: Customer Purchase History Completeness
  - Problem 5.3: Multi-Currency Revenue Consolidation
  - Problem 5.4: Store Performance with Demographics

- [[Module-06-Query-Organization|Module 6: Query Organization]]
  - Problem 6.1: Above-Average Products
  - Problem 6.2: Top 3 Customers Per Country
  - Problem 6.3: Strong Growth Months
  - Problem 6.4: Customer Cohort Analysis

### Advanced Level (Weeks 3)

- [[Module-07-Advanced-Analytics|Module 7: Advanced Analytics]]
  - Problem 7.1: Monthly Product Rankings Within Category
  - Problem 7.2: Moving Average Revenue
  - Problem 7.3: Product Performance Percentiles
  - Problem 7.4: Customer Revenue Contribution

- [[Module-08-Multi-Dimensional-Analysis|Module 8: Multi-Dimensional Analysis]]
  - Problem 8.1: Hierarchical Sales Report
  - Problem 8.2: Product Analysis Across Dimensions
  - Problem 8.3: Geographic Report with Subtotals

- [[Module-09-Performance-Optimization|Module 9: Performance & Optimization]]
  - Problem 9.1: Analyzing Slow Queries
  - Problem 9.2: Date Filtering Problems
  - Problem 9.3: Optimizing Complex Joins

### Expert Level (Weeks 4)

- [[Module-10-Data-Quality|Module 10: Data Quality & Integrity]]
  - Problem 10.1: Finding Orphaned Records
  - Problem 10.2: Duplicate Detection
  - Problem 10.3: Data Completeness Check
  - Problem 10.4: Business Rule Validation

- [[Module-11-Advanced-Business-Logic|Module 11: Advanced Business Logic]]
  - Problem 11.1: Product Affinity Analysis (Market Basket)
  - Problem 11.2: Customer Churn Risk Scoring
  - Problem 11.3: Price Elasticity Analysis
  - Problem 11.4: ABC Inventory Classification

- [[Module-12-Expert-Challenges|Module 12: Expert Challenges]]
  - Problem 12.1: CLV Prediction Feature Engineering
  - Problem 12.2: Cohort Retention Matrix
  - Problem 12.3: Geographic Expansion Scoring
  - Problem 12.4: Real-Time Anomaly Detection
  - Problem 12.5: Multi-Touch Attribution

---

## 📚 Reference Materials

- [[Appendix-Database-Schema|Appendix: Database Schema]]
- [[Tips-For-Success|Tips for Success]]
- [[Testing-Your-Queries|Testing Your Queries]]
- [[Understanding-Results|Understanding Your Results]]
- [[Connection-Guide|Connecting to the Database]]
- [[Submission-Guidelines|Submission Guidelines]]
- [[Getting-Help|Getting Help]]

---

## 🎯 Learning Path

> [!success] Recommended Progression
> 
> **Weeks 1: Modules 1-3 (Beginner)**
> - Master basic SELECT, WHERE, GROUP BY
> - Practice simple joins
> - Get comfortable with aggregations
> 
> **Weeks 2: Modules 4-6 (Intermediate)**
> - Time series and trends
> - Multiple joins
> - Subqueries and CTEs
> 
> **Weeks 3: Modules 7-9 (Advanced)**
> - Window functions
> - Complex aggregations
> - Performance optimization
> 
> **Weeks 4: Modules 10-12 (Expert)**
> - Data quality
> - Complex business logic
> - Real-world challenges

---

## 🚀 Getting Started

1. Read the [[Connection-Guide|connection guide]] to set up your database access
2. Review the [[Appendix-Database-Schema|database schema]] to understand table relationships
3. Start with [[Module-01-Executive-Dashboard|Module 1]] and work through sequentially
4. Check [[Tips-For-Success|tips for success]] when you get stuck
5. Follow the [[Submission-Guidelines|submission guidelines]] for each problem

> [!quote] Remember
> SQL is a tool to answer business questions - focus on "why" not just "how". Start simple, add complexity, and always verify your results make business sense.

**Good luck!** 🚀

---

← [[00-Index|Home]] | Next: [[Module-01-Executive-Dashboard|Module 1: Executive Dashboard]] →