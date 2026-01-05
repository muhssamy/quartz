# Module 9: Performance & Optimization

> [!quote] Business Context
> **From**: Database Administrator  
> **To**: Analytics Team  
> **Need**: Your queries are running too slow! We need to understand why and fix them. Learn to write efficient SQL that doesn't bog down the database.

---

## Problem 9.1: Analyzing Slow Queries

> [!question] The Problem
> **"This customer revenue query takes 30 seconds. Why?"**

**Given this query:**

```sql
SELECT 
    c.customerkey,
    c.givenname,
    c.surname,
    SUM(s.quantity * s.netprice) as total_revenue
FROM customer c
JOIN sales s ON c.customerkey = s.customerkey
GROUP BY c.customerkey, c.givenname, c.surname
ORDER BY total_revenue DESC;
```

**📋 Your Tasks:**

1. Run EXPLAIN ANALYZE on this query
2. Identify what's slow (Sequential Scan? No indexes?)
3. Suggest improvements (indexes? rewriting?)
4. Test if your improvements help

> [!tip] What to Look For
> - Sequential Scans (reading every row) = BAD
> - Index Scans = GOOD
> - Hash Joins vs Nested Loops
> - Expensive operations (sorts, aggregations)

---

## Problem 9.2: Date Filtering Problems

> [!question] The Problem
> **"Monthly reports are timing out!"**

**Problematic query pattern:**

```sql
WHERE EXTRACT(MONTH FROM orderdate) = 6
  AND EXTRACT(YEAR FROM orderdate) = 2016
```

**📋 Your Tasks:**

1. Explain why this is slow
2. Rewrite it to be faster
3. Understand why the rewrite helps

> [!warning] The Issue
> When you use functions on columns (like EXTRACT), the database can't use indexes efficiently!

> [!tip] The Solution
> Instead of extracting month/year, use date ranges:
> - orderdate >= 'start of June'
> - AND orderdate < 'start of July'

---

## Problem 9.3: Optimizing Complex Joins

> [!question] The Problem
> **"The sales detail report joining 5 tables is unbearably slow."**

**📋 Consider:**

- Does JOIN order matter?
- Are there indexes on join columns?
- Are you joining more tables than necessary?
- Could you filter earlier in the query?

**Your Tasks:**

1. Analyze the execution plan
2. Suggest index strategies
3. Consider if query can be restructured

> [!note] Performance Principles
> - Join smallest tables first when possible
> - Filter early (WHERE clause reduces rows)
> - Index foreign key columns
> - Don't SELECT columns you don't need

---

## 💡 Module Summary

In this module, you learned to:
- Use EXPLAIN ANALYZE to diagnose slow queries
- Understand execution plans and identify bottlenecks
- Optimize date filtering for index usage
- Design effective indexing strategies
- Rewrite queries for better performance
- Apply query optimization best practices

---

← [[Module-08-Multi-Dimensional-Analysis|← Previous: Module 8]] | [[00-Index|Home]] | [[Module-10-Data-Quality|Next: Module 10 →]]