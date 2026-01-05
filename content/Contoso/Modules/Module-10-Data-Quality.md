# Module 10: Data Quality & Integrity

> [!quote] Business Context
> **From**: Data Governance Team  
> **To**: Analytics Team  
> **Need**: We have data quality issues causing wrong reports and bad business decisions. We need to identify and quantify these problems.

---

## Problem 10.1: Finding Orphaned Records

> [!question] The Business Question
> **"Do we have sales pointing to non-existent customers or products?"**
> 
> Data integrity violations mean we can't trust our reports.

**📋 What You Need to Find:**

Identify all violations:

- Sales with customer keys that don't exist in customer table
- Sales with product keys that don't exist in product table
- Orders without any order rows
- Quantify the impact (how much revenue affected?)

> [!warning] Why This Matters
> If you have sales with invalid product keys:
> - You can't join to get product names
> - Category reports will be wrong
> - You don't know what actually sold!

---

## Problem 10.2: Duplicate Detection

> [!question] The Business Question
> **"Do we have duplicate customer or product records?"**
> 
> Duplicates mess up counts and analysis.

**📋 What You Need to Find:**

Look for:

- Customers with identical name + address (different IDs)
- Products with identical names (different product keys)
- Any duplicate order keys

> [!tip] Detecting Duplicates
> - Group by the identifying fields (name, address)
> - Count how many records per group
> - Groups with COUNT > 1 are duplicates
> - Show which specific records are duplicates

---

## Problem 10.3: Data Completeness Check

> [!question] The Business Question
> **"How much of our data is missing or NULL?"**
> 
> Understanding data quality: which fields have lots of NULLs?

**📋 What You Need to Report:**

For key tables and columns:

- What percentage is NULL?
- Date range coverage (first date, last date, any gaps?)
- Volume trends by month (are we getting more or less data?)

> [!example] What You Might Find
> - 15% of customers have NULL birthday
> - 5% of sales have NULL customer key (BIG PROBLEM!)
> - Sales data exists 2016-2020, but nothing for 2021 (missing year!)

---

## Problem 10.4: Business Rule Validation

> [!question] The Business Question
> **"Are transactions following business rules?"**
> 
> Sometimes data violates basic logic - we need to find these problems.

**📋 Check These Business Rules:**

Find violations where:

- Net Price > Unit Price (discount should make it lower!)
- Delivery Date < Order Date (delivered before ordered?!)
- Exchange Rate ≤ 0 (impossible!)
- Quantity ≤ 0 (negative or zero quantity?!)
- Unit Cost > Unit Price (selling at a loss on every item!)

> [!note] Why Rules Matter
> Each violation means either:
> - Data entry error
> - System bug
> - Actual fraud/abuse
> 
> All need investigation!

---

## 💡 Module Summary

In this module, you learned to:
- Identify orphaned records using anti-joins
- Detect duplicate records
- Assess data completeness and NULL percentages
- Validate business rules with logic checks
- Quantify data quality issues
- Write queries to monitor data integrity

---

← [[Module-09-Performance-Optimization|← Previous: Module 9]] | [[00-Index|Home]] | [[Module-11-Advanced-Business-Logic|Next: Module 11 →]]