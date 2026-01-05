# Module 8: Multi-Dimensional Analysis

> [!quote] Business Context
> **From**: Executive Team  
> **To**: BI Team  
> **Need**: We need reports with subtotals and totals at multiple levels - like Excel pivot tables but in SQL. Grand totals, subtotals by year, subtotals by quarter, etc.

---

## Problem 8.1: Hierarchical Sales Report

> [!question] The Business Question
> **"Create an executive summary with multiple levels of totals."**
> 
> One report showing: grand total, yearly totals, quarterly totals, and monthly detail.

**📋 What You Need to Deliver:**

Show revenue with hierarchical grouping:

- Detail: Year + Quarter + Month (e.g., "2016, Q1, January")
- Subtotal: Year + Quarter only (e.g., "2016, Q1, [Quarter Total]")
- Subtotal: Year only (e.g., "2016, [Year Total]")
- Grand Total: Everything

All in ONE result set!

> [!example] What The Output Looks Like
> ```
> Year | Quarter | Month    | Revenue
> 2016 | Q1      | January  | $4.5M     ← Detail
> 2016 | Q1      | February | $4.9M     ← Detail  
> 2016 | Q1      | March    | $4.4M     ← Detail
> 2016 | Q1      | NULL     | $13.8M    ← Quarter Subtotal
> 2016 | Q2      | April    | $4.7M     ← Detail
> ...
> 2016 | NULL    | NULL     | $54.2M    ← Year Subtotal
> NULL | NULL    | NULL     | $125.8M   ← Grand Total
> ```

> [!tip] Hints
> - There are special SQL features for this: ROLLUP, CUBE, or GROUPING SETS
> - Or you could UNION multiple queries together (detail, subtotals, grand total)
> - NULLs indicate "all" at that level

---

## Problem 8.2: Product Analysis Across Dimensions

> [!question] The Business Question
> **"Analyze products by category, brand, and combinations of both."**
> 
> We want to see revenue by category alone, by brand alone, by category+brand, AND grand total.

**📋 What You Need to Deliver:**

All of these views in one result:

- Revenue by Category (all brands combined)
- Revenue by Brand (all categories combined)
- Revenue by Category + Brand combination
- Grand Total (everything)

> [!note] Why Multiple Dimensions?
> - "Audio category" revenue = all brands of audio
> - "Contoso brand" revenue = Contoso products across all categories
> - "Contoso Audio" = intersection of both
> - This gives complete picture from all angles

---

## Problem 8.3: Geographic Report with Subtotals

> [!question] The Business Question
> **"Create a formatted report showing Country > State > City with subtotals."**
> 
> Like a hierarchy: USA total > California total > Los Angeles detail

**📋 What You Need to Deliver:**

Revenue hierarchy:

- Detail: Country + State + City (e.g., "USA, California, Los Angeles")
- Subtotal: Country + State (e.g., "USA, California, [State Total]")
- Subtotal: Country (e.g., "USA, [Country Total]")
- Grand Total

With nice labels like "Total USA", "Total California", etc.

> [!tip] Making It Pretty
> - Use CASE to detect subtotal rows
> - Add descriptive labels instead of just showing NULLs
> - "Total California" is better than "California | NULL"

---

## 💡 Module Summary

In this module, you learned to:
- Use ROLLUP, CUBE, and GROUPING SETS
- Create hierarchical reports with subtotals
- Analyze data across multiple dimensions
- Format reports with meaningful labels
- Build pivot-table-style outputs in SQL

---

← [[Module-07-Advanced-Analytics|← Previous: Module 7]] | [[00-Index|Home]] | [[Module-09-Performance-Optimization|Next: Module 9 →]]