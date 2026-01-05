# Testing Your Queries

## 🧪 Sanity Checks

> [!tip] Always Ask These Questions Before Submitting
> - Does the row count make sense?
> - Are the numbers reasonable?
> - Do percentages add to 100% (when they should)?
> - Are dates in valid range?
> - Do joins multiply rows unexpectedly?

---

## ✅ Pre-Flight Checklist

### 1. Check Row Counts

```sql
-- Know your baseline counts
SELECT 'customers' as table_name, COUNT(*) as row_count FROM customer
UNION ALL
SELECT 'products', COUNT(*) FROM product
UNION ALL
SELECT 'stores', COUNT(*) FROM store
UNION ALL
SELECT 'sales', COUNT(*) FROM sales
UNION ALL
SELECT 'orders', COUNT(*) FROM orders;
```

**Expected Results:**
- Customers: ~2,000,000
- Products: ~2,500
- Stores: ~200
- Sales: ~20,000,000
- Orders: ~5,000,000

> [!warning] Red Flag
> If your query returns 40M rows from sales (should be 20M), you probably have a bad join creating duplicates!

---

### 2. Verify Number Ranges

```sql
-- Check if numbers are reasonable
SELECT 
    MIN(quantity) as min_qty,
    MAX(quantity) as max_qty,
    AVG(quantity) as avg_qty,
    MIN(unitprice) as min_price,
    MAX(unitprice) as max_price,
    AVG(unitprice) as avg_price
FROM sales;
```

**What to Look For:**
- ❌ Quantity = -999999 (probably error)
- ❌ Price = $0 (might be valid, but investigate)
- ❌ Price = $999,999,999 (probably error)
- ✅ Quantity between 1-100 (reasonable)
- ✅ Price between $10-$2000 (reasonable for this dataset)

---

### 3. Check Date Ranges

```sql
-- Verify dates are in expected range
SELECT 
    MIN(orderdate) as first_date,
    MAX(orderdate) as last_date,
    MAX(orderdate) - MIN(orderdate) as days_span
FROM sales;
```

**Expected for Contoso Database:**
- First date: ~2016-01-01
- Last date: ~2027-12-31
- Span: ~4,383 days (12 years)

> [!warning] Red Flags
> - Dates in 1900 or 2099 (likely data errors)
> - Future dates beyond 2027
> - Dates before 2016

---

### 4. Validate Percentages

```sql
-- When calculating percentages, check they sum to 100%
SELECT 
    categoryname,
    COUNT(*) as product_count,
    ROUND(COUNT(*) * 100.0 / SUM(COUNT(*)) OVER (), 2) as percentage
FROM product
GROUP BY categoryname;

-- This should sum to 100%
SELECT SUM(percentage) FROM (
    SELECT 
        categoryname,
        ROUND(COUNT(*) * 100.0 / SUM(COUNT(*)) OVER (), 2) as percentage
    FROM product
    GROUP BY categoryname
) subq;
```

---

### 5. Check for NULLs

```sql
-- Identify NULL values that might affect your results
SELECT 
    COUNT(*) as total_rows,
    COUNT(customerkey) as non_null_customer,
    COUNT(productkey) as non_null_product,
    COUNT(storekey) as non_null_store,  -- Online orders have NULL store
    COUNT(*) - COUNT(storekey) as null_store_count
FROM sales;
```

> [!note] NULLs in Sales
> - NULL storekey = online order (this is normal!)
> - NULL customerkey = BIG PROBLEM (every sale should have customer)
> - NULL productkey = BIG PROBLEM (every sale should have product)

---

## 🔍 Testing Joins

### Test Join Results

```sql
-- BEFORE joining, count rows
SELECT COUNT(*) FROM sales;  -- Should be ~20M

-- AFTER joining, count should be same (if INNER JOIN)
SELECT COUNT(*) 
FROM sales s
JOIN product p ON s.productkey = p.productkey;  -- Should still be ~20M

-- If row count doubled, you have duplicate products!
```

### Detect Cartesian Products

```sql
-- Bad join (missing ON clause creates Cartesian product)
SELECT COUNT(*) 
FROM sales s, customer c;  -- Returns 20M * 2M = 40 TRILLION rows!

-- Check for accidental many-to-many
SELECT 
    s.saleskey,
    COUNT(*) as duplicate_count
FROM sales s
JOIN product p ON s.productkey = p.productkey
GROUP BY s.saleskey
HAVING COUNT(*) > 1;  -- Should return 0 rows
```

---

## 🧮 Validate Calculations

### Revenue Calculations

```sql
-- Test on small sample first
SELECT 
    saleskey,
    quantity,
    netprice,
    unitcost,
    quantity * netprice as revenue,
    quantity * unitcost as cost,
    (quantity * netprice) - (quantity * unitcost) as profit,
    ROUND(
        ((quantity * netprice) - (quantity * unitcost)) * 100.0 
        / NULLIF(quantity * netprice, 0), 
        2
    ) as margin_pct
FROM sales
LIMIT 10;
```

**Manually Verify:**
- Pick one row and calculate by hand
- Revenue should be positive (usually)
- Cost should be less than revenue (usually)
- Margin should be between 0% and 100% (usually)

### Aggregation Tests

```sql
-- Sum should equal grand total
WITH category_totals AS (
    SELECT 
        categoryname,
        SUM(quantity * netprice) as category_revenue
    FROM sales s
    JOIN product p ON s.productkey = p.productkey
    GROUP BY categoryname
)
SELECT 
    SUM(category_revenue) as sum_of_categories,
    (SELECT SUM(quantity * netprice) FROM sales) as grand_total,
    -- These should match!
    CASE 
        WHEN ABS(SUM(category_revenue) - (SELECT SUM(quantity * netprice) FROM sales)) < 0.01 
        THEN 'PASS' 
        ELSE 'FAIL' 
    END as test_result
FROM category_totals;
```

---

## 🚦 Testing Workflow

### Step-by-Step Testing Process

**1. Start Small**
```sql
-- Test on limited data first
SELECT * FROM sales LIMIT 10;
```

**2. Add One Piece at a Time**
```sql
-- Add calculation
SELECT 
    saleskey,
    quantity * netprice as revenue 
FROM sales 
LIMIT 10;
```

**3. Add Joins Gradually**
```sql
-- Add first join
SELECT 
    s.saleskey,
    s.quantity * s.netprice as revenue,
    p.productname
FROM sales s
JOIN product p ON s.productkey = p.productkey
LIMIT 10;
```

**4. Verify Row Count After Each Join**
```sql
-- Before join
SELECT COUNT(*) FROM sales;  -- Note this number

-- After join
SELECT COUNT(*) 
FROM sales s
JOIN product p ON s.productkey = p.productkey;  -- Should be same
```

**5. Add Aggregation**
```sql
-- Remove LIMIT, add GROUP BY
SELECT 
    p.categoryname,
    SUM(s.quantity * s.netprice) as revenue
FROM sales s
JOIN product p ON s.productkey = p.productkey
GROUP BY p.categoryname;
```

**6. Add Filters Last**
```sql
-- Add WHERE clause
SELECT 
    p.categoryname,
    SUM(s.quantity * s.netprice) as revenue
FROM sales s
JOIN product p ON s.productkey = p.productkey
WHERE s.orderdate >= '2016-01-01'
GROUP BY p.categoryname;
```

---

## 🎯 Common Testing Scenarios

### Test Case 1: Customer With No Orders

```sql
-- Some customers might have never ordered
-- If using INNER JOIN, they won't appear

-- This might return 0
SELECT COUNT(DISTINCT customerkey) FROM sales;  -- Customers who ordered

-- This should be higher
SELECT COUNT(*) FROM customer;  -- All customers

-- Use LEFT JOIN to include all customers
SELECT 
    c.customerkey,
    c.givenname,
    COUNT(s.saleskey) as order_count
FROM customer c
LEFT JOIN sales s ON c.customerkey = s.customerkey
GROUP BY c.customerkey, c.givenname
HAVING COUNT(s.saleskey) = 0  -- Never ordered
LIMIT 10;
```

### Test Case 2: Products Never Sold

```sql
-- Similar issue with products
SELECT COUNT(*) FROM product;  -- All products: ~2,500

SELECT COUNT(DISTINCT productkey) FROM sales;  -- Sold products: ~2,300

-- Find the unsold products
SELECT 
    p.productkey,
    p.productname,
    COALESCE(SUM(s.quantity), 0) as units_sold
FROM product p
LEFT JOIN sales s ON p.productkey = s.productkey
GROUP BY p.productkey, p.productname
HAVING COALESCE(SUM(s.quantity), 0) = 0;
```

### Test Case 3: Divide by Zero

```sql
-- This will error if any customer has 0 orders
SELECT 
    customerkey,
    total_revenue / order_count as avg_order_value  -- ERROR!
FROM customer_summary
WHERE order_count = 0;

-- Fix with NULLIF or CASE
SELECT 
    customerkey,
    total_revenue / NULLIF(order_count, 0) as avg_order_value  -- Returns NULL
FROM customer_summary;
```

---

## 📊 Output Validation

### Visual Inspection

> [!tip] Look for These Patterns
> - **Consistent data types**: All dates look like dates, numbers like numbers
> - **No weird characters**: No ??? or □ symbols
> - **Reasonable precision**: Money rounded to 2 decimals
> - **Sorted meaningfully**: Results in logical order

### Example Good Output

```sql
SELECT 
    p.categoryname,
    COUNT(DISTINCT p.productkey) as product_count,
    ROUND(SUM(s.quantity * s.netprice), 2) as revenue,
    ROUND(AVG(s.quantity), 1) as avg_quantity
FROM sales s
JOIN product p ON s.productkey = p.productkey
GROUP BY p.categoryname
ORDER BY revenue DESC;
```

**Expected Result:**
```
categoryname    | product_count | revenue      | avg_quantity
Computers       | 125           | 45,234,567.89| 2.3
Audio           | 98            | 32,456,789.12| 1.8
Cameras         | 76            | 28,765,432.10| 1.5
```

✅ **Good:** Clean, aligned, rounded, sorted

### Example Bad Output

```
categoryname|product_count|revenue|avg_quantity
Computers|125|45234567.8932847|2.283749287
NULL|0|NULL|NULL
Audio|98|32456789.119999|1.8
```

❌ **Bad:** Too many decimals, NULLs not handled, not sorted

---

## 🐛 Debugging Checklist

When your query doesn't work:

- [ ] Did I spell table/column names correctly?
- [ ] Are my JOINs using the right keys?
- [ ] Did I include all GROUP BY columns that aren't aggregated?
- [ ] Are my parentheses balanced?
- [ ] Did I handle NULL values?
- [ ] Did I use the right join type (INNER vs LEFT)?
- [ ] Are my date formats correct?
- [ ] Did I test on a small sample first (LIMIT)?
- [ ] Have I checked for duplicates after joining?
- [ ] Do my calculations make mathematical sense?

---

← [[00-Index|Back to Home]]