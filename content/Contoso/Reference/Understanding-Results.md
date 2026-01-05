# Understanding Your Results

## 📊 Business Context for Numbers

### Revenue Ranges (Contoso Database)

**Daily Revenue:**
- Typical: $50,000 - $150,000
- Weekends: Higher (up to $200,000)
- Holidays: Much higher (up to $300,000)
- Slow days: As low as $30,000

**Monthly Revenue:**
- Typical: $1.5M - $5M
- Q4 (holiday season): $6M - $8M
- Q1 (post-holiday): $1M - $2M

**Annual Revenue:**
- Typical: $25M - $60M per year
- Growth rate: 5-15% year-over-year

> [!warning] Red Flags
> - Daily revenue > $500K (possible error or major event)
> - Monthly revenue < $500K (missing data?)
> - Annual revenue > $100M (check your calculations)

---

### Customer Metrics

**Order Value:**
- Average order: $300 - $800
- Small orders: $50 - $200 (single item purchases)
- Large orders: $1,000 - $5,000 (bulk purchases)
- Extreme orders: > $10,000 (B2B customers or data errors?)

**Orders Per Customer:**
- One-time buyers: 1 order (40-50% of customers)
- Occasional: 2-5 orders (30-40% of customers)
- Regular: 6-10 orders (10-15% of customers)
- Loyal: 10+ orders (5-10% of customers)
- VIP: 50+ orders (< 1% of customers)

**Customer Lifetime Value:**
- Typical: $500 - $2,000
- Good customers: $2,000 - $5,000
- VIP customers: $5,000 - $20,000
- Extreme outliers: > $50,000 (investigate!)

**Customer Age:**
- Range: 18 - 90 years
- Average: 35 - 45 years
- Peak buying age: 30 - 50 years

> [!note] What's Normal?
> - 80% of customers have 1-3 orders (this is normal!)
> - Top 20% of customers generate ~80% of revenue (Pareto principle)
> - Average customer value: $800 - $1,500

---

### Product Metrics

**Product Prices:**
- Budget items: $10 - $50 (accessories, games)
- Mid-range: $50 - $500 (audio, small appliances)
- Premium: $500 - $2,000 (computers, TVs)
- Extreme: > $2,000 (high-end electronics)

**Profit Margins:**
- Low margin: 20-30% (TVs, computers - competitive)
- Target margin: 35-50% (most products)
- High margin: 50-70% (accessories, cables)
- Extreme margin: > 70% (check for errors)

**Units Per Sale:**
- Typical: 1-3 units per transaction
- Bulk: 5-10 units
- Suspicious: > 100 units (data error or B2B order)

**Sales Velocity:**
- Fast movers: 100+ units/day
- Regular: 10-50 units/day
- Slow movers: 1-10 units/day
- Dead stock: < 10 units/year

---

## 🔍 Interpreting Your Results

### Scenario 1: Getting One Row When Expecting Many

**Your Query:**
```sql
SELECT 
    productname,
    SUM(quantity * netprice) as revenue
FROM sales s
JOIN product p ON s.productkey = p.productkey;
-- Missing GROUP BY!
```

**Result:** One row with grand total

**What Happened:** Without GROUP BY, SQL aggregates everything into one row

**Fix:** Add GROUP BY for the columns you want to see:
```sql
GROUP BY p.productkey, p.productname
```

---

### Scenario 2: Too Many Rows

**Expected:** 2,500 products  
**Got:** 20,000,000 rows

**Your Query:**
```sql
SELECT 
    p.productname,
    s.quantity
FROM product p, sales s;  -- Missing JOIN condition!
```

**What Happened:** Cartesian product - every product matched with every sale

**Fix:** Add proper JOIN condition:
```sql
FROM product p
JOIN sales s ON p.productkey = s.productkey
```

---

### Scenario 3: All NULLs in Results

**Your Query:**
```sql
SELECT 
    p.productname,
    SUM(s.quantity) as units_sold
FROM product p
JOIN sales s ON p.productkey = s.productkey
GROUP BY p.productname
ORDER BY units_sold;
```

**First few rows are NULL?**

**What Happened:** Some products never sold, but you used INNER JOIN

**Fix:** Use LEFT JOIN to include products with no sales:
```sql
FROM product p
LEFT JOIN sales s ON p.productkey = s.productkey
GROUP BY p.productname
ORDER BY COALESCE(SUM(s.quantity), 0);
```

---

### Scenario 4: Wrong Totals

**Expected:** $50M total revenue  
**Got:** $500B total revenue

**Possible Causes:**

**1. Multiplying joined tables:**
```sql
-- Wrong: Counting each sale multiple times
SELECT SUM(quantity * netprice) as revenue
FROM sales s
JOIN orderrows r ON s.orderkey = r.orderkey;
-- Each sale gets counted once per order row!
```

**2. Not using DISTINCT:**
```sql
-- Wrong: Counting same order multiple times
SELECT COUNT(orderkey) as orders FROM sales;
-- Should be: COUNT(DISTINCT orderkey)
```

**3. Including returns as sales:**
```sql
-- May need to exclude returns
WHERE quantity > 0
```

---

### Scenario 5: Percentages Don't Add to 100%

**Your Query:**
```sql
SELECT 
    categoryname,
    COUNT(*) as product_count,
    ROUND(COUNT(*) / SUM(COUNT(*)), 2) as percentage
FROM product
GROUP BY categoryname;
```

**Problem:** Integer division gives 0

**Fix:** Use floating point:
```sql
ROUND(COUNT(*) * 100.0 / SUM(COUNT(*)) OVER (), 2) as percentage
```

Or use decimal cast:
```sql
ROUND(COUNT(*)::decimal / SUM(COUNT(*)) OVER () * 100, 2)
```

---

## 📈 Validating Your Results

### The "Smell Test"

Ask yourself these questions:

**Do the numbers make business sense?**
- ❌ Average order value: $50,000 (too high!)
- ✅ Average order value: $450 (reasonable)

**Are proportions reasonable?**
- ❌ 80% profit margin (too good to be true)
- ✅ 45% profit margin (healthy and realistic)

**Do trends match reality?**
- ❌ Revenue doubles every month (unrealistic growth)
- ✅ Revenue grows 8% year-over-year (realistic)

**Are there impossible values?**
- ❌ Negative revenue (unless returns)
- ❌ Customer age: 150 years old
- ❌ Quantity: -5,000 (unless returns)

---

### Cross-Validation Techniques

**1. Check Totals Match**
```sql
-- Revenue by category should sum to total revenue
SELECT 
    SUM(category_revenue) as sum_of_categories,
    (SELECT SUM(quantity * netprice) FROM sales) as grand_total
FROM (
    SELECT 
        p.categoryname,
        SUM(s.quantity * s.netprice) as category_revenue
    FROM sales s
    JOIN product p ON s.productkey = p.productkey
    GROUP BY p.categoryname
) subq;
```

**2. Verify Row Counts**
```sql
-- Number of distinct customers should equal customer count
SELECT 
    COUNT(DISTINCT customerkey) as customers_with_sales,
    (SELECT COUNT(*) FROM customer) as total_customers;
```

**3. Check Date Ranges**
```sql
-- Make sure dates are in expected range
SELECT 
    MIN(orderdate) as earliest,
    MAX(orderdate) as latest,
    COUNT(DISTINCT DATE(orderdate)) as days_with_sales
FROM sales;
```

---

## 🎯 Common Patterns and What They Mean

### Pattern 1: Power Law Distribution (80/20 Rule)

```
Top 20% of customers → 80% of revenue
Top 20% of products → 80% of sales
```

**What it means:** This is NORMAL and expected in retail. A small number of customers/products drive most of the business.

**Business implications:**
- Focus on retaining top customers
- Ensure top products never go out of stock
- Consider "long tail" strategy for others

---

### Pattern 2: Seasonal Patterns

```
Q4 revenue: $8M
Q1 revenue: $2M
Q2 revenue: $3M
Q3 revenue: $4M
```

**What it means:** Holiday shopping season (Q4) drives significant sales

**Business implications:**
- Build inventory ahead of Q4
- Plan marketing campaigns for Q4
- Expect cash flow constraints in Q1

---

### Pattern 3: Customer Lifecycle

```
Month 0: 100% active (by definition)
Month 1: 45% return
Month 3: 30% still active
Month 6: 20% still active
Month 12: 15% still active
```

**What it means:** Rapid customer dropoff is normal, but retention matters

**Business implications:**
- Focus on Month 1 retention (win them back quickly)
- Loyalty programs for Month 3+ customers
- Identify at-risk customers in Month 1-2

---

### Pattern 4: Product Maturity Curve

```
New product: High growth, low volume
Mature product: Stable volume, predictable
Declining product: Decreasing sales
```

**What it means:** Products have lifecycles

**Business implications:**
- Promote new products heavily
- Maintain inventory for mature products
- Discount declining products to clear inventory

---

## 💡 When Results Look Wrong

### Checklist for Investigating

**1. Query Logic:**
- [ ] Are my JOINs correct?
- [ ] Did I use the right aggregation functions?
- [ ] Are my filters correct?
- [ ] Did I handle NULLs properly?

**2. Data Quality:**
- [ ] Are there duplicates in the source data?
- [ ] Are there NULL values affecting calculations?
- [ ] Are date ranges correct?
- [ ] Are there data entry errors?

**3. Business Logic:**
- [ ] Am I answering the right question?
- [ ] Are my assumptions correct?
- [ ] Did I include/exclude the right records?
- [ ] Are my calculations mathematically sound?

**4. Compare to Benchmarks:**
- [ ] How does this compare to last year?
- [ ] Is this in line with industry standards?
- [ ] Does it match other reports?
- [ ] Can someone verify these numbers?

---

## 📝 Documenting Assumptions

Always document assumptions in your analysis:

```sql
-- ASSUMPTIONS:
-- 1. NULL storekey indicates online orders
-- 2. Negative quantity indicates returns/refunds
-- 3. We exclude quantity = 0 as data errors
-- 4. Revenue is calculated using netprice (after discounts)
-- 5. Customer tenure is from first purchase to most recent data

SELECT 
    customerkey,
    MIN(orderdate) as first_purchase,
    MAX(orderdate) as last_purchase,
    MAX(orderdate) - MIN(orderdate) as tenure_days,
    SUM(CASE WHEN quantity > 0 THEN quantity * netprice ELSE 0 END) as revenue
FROM sales
WHERE storekey IS NULL  -- Online only
GROUP BY customerkey;
```

---

## 🎓 Learning to Trust Your Results

### Build Confidence Through:

**1. Start Small**
- Test queries on limited data first
- Verify results manually on small samples
- Gradually expand to full dataset

**2. Cross-Reference**
- Compare your results to similar analyses
- Check against known totals
- Validate with stakeholders

**3. Document Everything**
- Keep notes on your logic
- Explain your assumptions
- Track changes to queries

**4. Learn from Mistakes**
- When you find errors, understand why
- Document lessons learned
- Build better validation into future queries

---

← [[00-Index|Back to Home]]