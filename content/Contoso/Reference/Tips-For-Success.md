# Tips for Success

## 🎯 General SQL Problem-Solving Approach

> [!tip] Step-by-Step Process
> 1. **Understand the business question** - What does the business really need?
> 2. **Identify required tables** - Which tables have the data?
> 3. **Start simple** - Get basic query working first
> 4. **Add complexity gradually** - One piece at a time
> 5. **Test with LIMIT** - Use LIMIT 10 while developing
> 6. **Verify results** - Do the numbers make sense?
> 7. **Format for readability** - Use aliases, format numbers

---

## ✍️ Writing Clean SQL

### Good SQL Example
```sql
-- Good: Clear, readable, well-formatted
SELECT 
    c.givenname AS first_name,
    c.surname AS last_name,
    COUNT(DISTINCT s.orderkey) AS total_orders,
    SUM(s.quantity * s.netprice) AS total_revenue,
    ROUND(AVG(s.quantity * s.netprice), 2) AS avg_order_value
FROM customer c
JOIN sales s ON c.customerkey = s.customerkey
WHERE s.orderdate >= '2016-01-01'
    AND s.orderdate < '2017-01-01'
GROUP BY c.customerkey, c.givenname, c.surname
HAVING COUNT(DISTINCT s.orderkey) > 5
ORDER BY total_revenue DESC
LIMIT 100;
```

### What Makes This Good?
- ✅ Clear aliasing (c, s for tables; descriptive for columns)
- ✅ Proper indentation
- ✅ Comments explaining what/why
- ✅ Readable column names
- ✅ One clause per line
- ✅ Consistent formatting

### Bad SQL Example
```sql
-- Bad: Hard to read, no formatting
select c.givenname,c.surname,count(distinct s.orderkey),sum(s.quantity*s.netprice) from customer c join sales s on c.customerkey=s.customerkey where s.orderdate>='2016-01-01' group by c.customerkey,c.givenname,c.surname having count(distinct s.orderkey)>5 order by sum(s.quantity*s.netprice) desc;
```

---

## ⚠️ Common Mistakes to Avoid

### 1. NULL Values
```sql
-- ❌ WRONG: Doesn't handle NULLs
SELECT COUNT(birthdate) FROM customer;

-- ✅ CORRECT: Be explicit about NULLs
SELECT 
    COUNT(*) as total_customers,
    COUNT(birthdate) as customers_with_birthdate,
    COUNT(*) - COUNT(birthdate) as missing_birthdate
FROM customer;
```

### 2. Wrong Join Type
```sql
-- ❌ WRONG: Only shows products that sold
SELECT p.productname, SUM(s.quantity) as units_sold
FROM product p
JOIN sales s ON p.productkey = s.productkey
GROUP BY p.productkey, p.productname;

-- ✅ CORRECT: Shows ALL products (even with 0 sales)
SELECT 
    p.productname, 
    COALESCE(SUM(s.quantity), 0) as units_sold
FROM product p
LEFT JOIN sales s ON p.productkey = s.productkey
GROUP BY p.productkey, p.productname;
```

### 3. Divide by Zero
```sql
-- ❌ WRONG: Will error if denominator is 0
SELECT 
    revenue / orders as avg_order_value
FROM sales_summary;

-- ✅ CORRECT: Handle zero case
SELECT 
    CASE 
        WHEN orders > 0 THEN revenue / orders 
        ELSE 0 
    END as avg_order_value
FROM sales_summary;
```

### 4. Not Filtering Early
```sql
-- ❌ SLOW: Joins all data first, then filters
SELECT ...
FROM sales s
JOIN customer c ON s.customerkey = c.customerkey
WHERE s.orderdate >= '2016-01-01';

-- ✅ FASTER: Filter in subquery first
SELECT ...
FROM (
    SELECT * FROM sales 
    WHERE orderdate >= '2016-01-01'
) s
JOIN customer c ON s.customerkey = c.customerkey;
```

### 5. Count vs Count Distinct
```sql
-- ❌ WRONG: Counts rows, not unique orders
SELECT customerkey, COUNT(orderkey) as orders
FROM sales
GROUP BY customerkey;

-- ✅ CORRECT: Counts distinct orders
SELECT customerkey, COUNT(DISTINCT orderkey) as orders
FROM sales
GROUP BY customerkey;
```

### 6. Forgetting to Round
```sql
-- ❌ UGLY: Too many decimals
SELECT AVG(unitprice) FROM sales;
-- Result: 245.8392847392847

-- ✅ CLEAN: Rounded nicely
SELECT ROUND(AVG(unitprice), 2) FROM sales;
-- Result: 245.84
```

---

## 🔍 Testing Your Logic

### Always Ask Yourself:

**Does the row count make sense?**
```sql
-- Quick sanity check
SELECT COUNT(*) FROM product;  -- Should be ~2,500
SELECT COUNT(*) FROM customer;  -- Should be ~2M
SELECT COUNT(*) FROM sales;     -- Should be ~20M
```

**Are the numbers reasonable?**
```sql
-- Check for outliers
SELECT 
    MIN(quantity) as min_qty,
    MAX(quantity) as max_qty,
    AVG(quantity) as avg_qty
FROM sales;

-- If max_qty is 1,000,000 - probably an error!
```

**Do percentages add up?**
```sql
-- When grouping, percentages should sum to 100%
SELECT 
    categoryname,
    COUNT(*) as count,
    ROUND(COUNT(*) * 100.0 / SUM(COUNT(*)) OVER (), 2) as pct
FROM product
GROUP BY categoryname;
```

**Are dates in valid range?**
```sql
-- Check date boundaries
SELECT 
    MIN(orderdate) as first_order,
    MAX(orderdate) as last_order
FROM sales;
-- Should be 2016-2027 for this database
```

---

## 🐛 Debugging Techniques

### 1. Break Complex Queries Into Parts
```sql
-- Instead of one giant query, use CTEs
WITH monthly_revenue AS (
    SELECT 
        DATE_TRUNC('month', orderdate) as month,
        SUM(quantity * netprice) as revenue
    FROM sales
    GROUP BY month
)
-- Test this CTE first!
SELECT * FROM monthly_revenue LIMIT 10;

-- Then add more CTEs
, growth AS (
    SELECT 
        month,
        revenue,
        LAG(revenue) OVER (ORDER BY month) as prev_revenue
    FROM monthly_revenue
)
SELECT * FROM growth;
```

### 2. Add Row Counts at Each Step
```sql
-- See how many rows at each stage
SELECT COUNT(*) FROM sales;  -- 20M
SELECT COUNT(*) FROM sales WHERE orderdate >= '2016-01-01';  -- 5M
SELECT COUNT(*) FROM sales s 
JOIN customer c ON s.customerkey = c.customerkey 
WHERE s.orderdate >= '2016-01-01';  -- Still 5M? Good!
```

### 3. Look at Sample Data
```sql
-- Always examine actual data
SELECT * FROM sales LIMIT 10;
SELECT * FROM customer WHERE customerkey = 123;
SELECT * FROM product WHERE productname LIKE '%Camera%';
```

### 4. Comment Out Parts
```sql
SELECT 
    c.givenname,
    COUNT(DISTINCT s.orderkey) as orders
    -- , SUM(s.quantity * s.netprice) as revenue  -- Comment out problem line
FROM customer c
JOIN sales s ON c.customerkey = s.customerkey
GROUP BY c.customerkey, c.givenname;
```

---

## 📊 Understanding Your Results

### Typical Value Ranges (Contoso Database)

**Revenue Metrics:**
- Daily revenue: $50K - $150K
- Monthly revenue: $1.5M - $5M
- Annual revenue: $25M - $60M

**Customer Metrics:**
- Average order value: $300 - $800
- Orders per customer: 1-5 (typically)
- Lifetime value: $500 - $2,000
- Customer age: 18-90 years

**Product Metrics:**
- Product prices: $10 - $2,000
- Profit margins: 30% - 60%
- Units per order: 1-5

**If Your Numbers Are Way Off:**
- Check your joins (Cartesian product?)
- Verify date filters
- Look for duplicates
- Ensure proper aggregation

---

## 💡 Pro Tips

### 1. Use Descriptive Aliases
```sql
-- ❌ Bad
SELECT a.b, a.c, d.e
FROM tbl1 a
JOIN tbl2 d ON a.x = d.y;

-- ✅ Good
SELECT 
    cust.givenname,
    cust.surname,
    ord.orderdate
FROM customer cust
JOIN sales ord ON cust.customerkey = ord.customerkey;
```

### 2. Build Up Complexity Gradually
```sql
-- Step 1: Just get data
SELECT * FROM sales LIMIT 10;

-- Step 2: Add calculation
SELECT quantity * netprice as revenue FROM sales LIMIT 10;

-- Step 3: Add aggregation
SELECT SUM(quantity * netprice) as total_revenue FROM sales;

-- Step 4: Add grouping
SELECT 
    DATE_TRUNC('month', orderdate) as month,
    SUM(quantity * netprice) as revenue
FROM sales
GROUP BY month;

-- Step 5: Add ordering
... ORDER BY month;
```

### 3. Document Your Assumptions
```sql
-- ASSUMPTION: We treat NULL customerkey as "guest checkout"
-- ASSUMPTION: Returns have negative quantity
-- ASSUMPTION: Revenue = quantity * netprice (after discounts)

SELECT 
    COALESCE(customerkey, 0) as customer_id,
    SUM(quantity * netprice) as revenue
FROM sales
WHERE quantity > 0  -- Exclude returns
GROUP BY customer_id;
```

### 4. Format Numbers for Readability
```sql
SELECT 
    ROUND(SUM(quantity * netprice), 2) as revenue,
    ROUND(SUM(quantity * unitcost), 2) as cost,
    ROUND(SUM(quantity * netprice) - SUM(quantity * unitcost), 2) as profit,
    ROUND(
        (SUM(quantity * netprice) - SUM(quantity * unitcost)) * 100.0 
        / NULLIF(SUM(quantity * netprice), 0), 
        2
    ) as profit_margin_pct
FROM sales;
```

---

## 🎓 Learning Resources

### When You're Stuck:

1. **Re-read the problem** - Are you answering the right question?
2. **Check the schema** - [[Appendix-Database-Schema|Database Schema Reference]]
3. **Look at examples** - Review similar problems you've solved
4. **Break it down** - What's the simplest version of this query?
5. **Ask for help** - [[Getting-Help|How to Get Help]]

### Practice Makes Perfect:

- Start with easier problems and work up
- Don't skip problems - each builds on previous skills
- Redo problems after a few days to reinforce learning
- Try to solve problems multiple ways
- Explain your solution to someone else (rubber duck debugging!)

---

← [[00-Index|Back to Home]]