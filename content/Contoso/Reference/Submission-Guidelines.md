# Submission Guidelines

## 📤 What to Submit

For each problem, you should submit:

1. ✅ **Your SQL query** (properly formatted)
2. ✅ **Sample output** (first 10-20 rows)
3. ✅ **Brief explanation** (What does your query do? Why this approach?)
4. ✅ **Business insight** (What did you learn from the results?)

---

## 📝 Submission Format

### Template

Use this template for each problem:

```markdown
# Problem X.Y: [Problem Title]

## SQL Query

[paste your SQL here]

## Sample Output

[paste first 10-20 rows of results]

## Explanation

[Explain what your query does and why you chose this approach]

## Business Insight

[What business insight did you gain from the results?]
```

---

## ✨ Example Good Submission

### Problem 2.1: Category Performance Ranking

#### SQL Query

```sql
-- Problem 2.1: Category Performance Ranking
-- This query analyzes revenue and profitability by product category
-- to help identify which categories deserve more investment.

SELECT 
    p.categoryname,
    COUNT(DISTINCT p.productkey) as product_count,
    SUM(s.quantity) as total_units_sold,
    ROUND(SUM(s.quantity * s.netprice), 2) as total_revenue,
    ROUND(AVG(s.netprice), 2) as avg_price_point,
    ROUND(
        (SUM(s.quantity * s.netprice) - SUM(s.quantity * s.unitcost)) * 100.0 
        / NULLIF(SUM(s.quantity * s.netprice), 0),
        2
    ) as profit_margin_pct
FROM sales s
JOIN product p ON s.productkey = p.productkey
GROUP BY p.categoryname
ORDER BY total_revenue DESC;
```

#### Sample Output

```
categoryname                    | product_count | total_units_sold | total_revenue  | avg_price_point | profit_margin_pct
Computers                       | 125           | 850,234          | 45,234,567.89  | 532.15          | 48.23
Audio                           | 98            | 1,245,678        | 32,456,789.12  | 26.05           | 62.15
Cameras and camcorders          | 76            | 456,789          | 28,765,432.10  | 629.84          | 45.67
TV and Video                    | 89            | 234,567          | 22,345,678.90  | 952.34          | 41.23
Home Appliances                 | 145           | 567,890          | 18,234,567.80  | 321.08          | 38.45
Cell phones                     | 56            | 345,678          | 15,678,901.20  | 453.67          | 35.78
Games and Toys                  | 234           | 789,012          | 12,345,678.90  | 15.64           | 52.34
Music, Movies and Audio Books   | 178           | 456,234          | 8,901,234.50   | 19.51           | 58.92
```

#### Explanation

My query joins the sales and product tables to analyze performance by category. I calculated:

1. **Product count**: Shows category diversity (how many different products)
2. **Units sold**: Volume indicator
3. **Revenue**: Primary success metric
4. **Average price**: Helps understand if category is budget or premium
5. **Profit margin**: Critical for investment decisions

I used NULLIF to prevent divide-by-zero errors in the margin calculation. The results are ordered by revenue to highlight top-performing categories.

#### Business Insight

**Key Findings:**

1. **Computers** leads in revenue ($45.2M) but has moderate margin (48%)
2. **Audio** has the highest profit margin (62%) despite being #2 in revenue
3. **Games and Toys** has high margin (52%) but low average price ($15.64) - volume business
4. **TV and Video** has lower margin (41%) and high price - competitive category

**Recommendations:**

- **Maintain** strong Computers inventory (high revenue, solid margin)
- **Invest** in Audio expansion (excellent margins, good revenue)
- **Consider** increasing Games and Toys marketing (high margin, opportunity for growth)
- **Review** TV and Video pricing strategy (margins below target)

The data shows we have a healthy mix of high-volume/low-margin and low-volume/high-margin categories, which provides business stability.

---

## 🎯 What Makes a Good Submission?

### ✅ SQL Query Quality

**Good:**
- Clean, readable formatting
- Meaningful aliases
- Comments explaining complex logic
- Proper indentation
- Handles edge cases (NULLs, divide by zero)

**Bad:**
- One long line
- No comments
- Confusing aliases (a, b, c)
- No error handling

### ✅ Output Quality

**Good:**
- First 10-20 rows shown
- Clean formatting (aligned columns)
- Numbers rounded appropriately
- Column headers are clear

**Bad:**
- Only showing 3 rows
- Unformatted paste
- Too many decimal places
- Cryptic column names

### ✅ Explanation Quality

**Good:**
- Explains the approach
- Justifies design decisions
- Notes any assumptions made
- Mentions alternative approaches considered

**Bad:**
- "This query gets the data"
- No explanation of logic
- Doesn't address the business question

### ✅ Business Insight Quality

**Good:**
- Specific findings from the data
- Answers the original business question
- Provides actionable recommendations
- Shows critical thinking

**Bad:**
- "The numbers look good"
- Just repeats what the query does
- No business context
- Generic statements

---

## 📊 Formatting Your SQL

### Good Formatting Example

```sql
-- Clear, readable, professional
SELECT 
    c.givenname AS first_name,
    c.surname AS last_name,
    c.countryregionname AS country,
    COUNT(DISTINCT s.orderkey) AS total_orders,
    SUM(s.quantity * s.netprice) AS total_spent,
    ROUND(AVG(s.quantity * s.netprice), 2) AS avg_order_value,
    MIN(s.orderdate) AS first_purchase,
    MAX(s.orderdate) AS last_purchase
FROM customer c
JOIN sales s ON c.customerkey = s.customerkey
WHERE s.orderdate >= '2016-01-01'
    AND s.orderdate < '2017-01-01'
GROUP BY c.customerkey, c.givenname, c.surname, c.countryregionname
HAVING COUNT(DISTINCT s.orderkey) >= 5
ORDER BY total_spent DESC
LIMIT 50;
```

### Bad Formatting Example

```sql
-- Hard to read, unprofessional
select c.givenname,c.surname,c.countryregionname,count(distinct s.orderkey),sum(s.quantity*s.netprice),avg(s.quantity*s.netprice),min(s.orderdate),max(s.orderdate) from customer c join sales s on c.customerkey=s.customerkey where s.orderdate>='2016-01-01' and s.orderdate<'2017-01-01' group by c.customerkey,c.givenname,c.surname,c.countryregionname having count(distinct s.orderkey)>=5 order by sum(s.quantity*s.netprice) desc limit 50;
```

---

## 🔍 Self-Review Checklist

Before submitting, ask yourself:

### SQL Query
- [ ] Is my query properly formatted and indented?
- [ ] Did I use meaningful aliases?
- [ ] Are there comments explaining complex parts?
- [ ] Does it handle edge cases (NULLs, zeros)?
- [ ] Have I tested it on the actual database?
- [ ] Is the output what the problem asks for?

### Output
- [ ] Did I include 10-20 sample rows?
- [ ] Is the output readable and aligned?
- [ ] Are numbers properly rounded?
- [ ] Do the results make business sense?

### Explanation
- [ ] Did I explain my approach?
- [ ] Did I justify design decisions?
- [ ] Did I note any assumptions?
- [ ] Is it clear and concise?

### Business Insight
- [ ] Did I answer the original business question?
- [ ] Are my insights specific and data-driven?
- [ ] Did I provide actionable recommendations?
- [ ] Does it show understanding of the business context?

---

## 📁 File Naming Convention

Use this naming pattern:

```
ModuleXX_ProblemY_YourName.sql
```

Examples:
```
Module01_Problem1_JohnDoe.sql
Module02_Problem3_JaneSmith.sql
Module07_Problem2_AliceJones.sql
```

For documentation (if submitting separately):
```
ModuleXX_ProblemY_YourName.md
```

---

## ⚠️ Common Submission Mistakes

### Mistake 1: Incomplete Output

❌ **Bad:**
```
Only showing 3 rows because query takes too long...
```

✅ **Good:**
```
[Shows 10-20 rows]
Note: Full result set has 2,456 rows. Showing first 20 for readability.
```

### Mistake 2: No Business Context

❌ **Bad:**
```
Business Insight: The query returned 500 rows.
```

✅ **Good:**
```
Business Insight: We identified 500 at-risk customers who haven't purchased in 90+ days but were previously active (5+ orders). This represents $2.3M in potential lost revenue if they churn. Recommend immediate win-back campaign.
```

### Mistake 3: Unexplained Design Choices

❌ **Bad:**
```
Explanation: I joined the tables and grouped them.
```

✅ **Good:**
```
Explanation: I used LEFT JOIN instead of INNER JOIN to include all products, even those with zero sales. This is important because the business needs to see which products aren't selling. I handled NULL sales values using COALESCE to show 0 instead of NULL.
```

### Mistake 4: Unreadable Query

❌ **Bad:**
```sql
select * from sales s,customer c where s.customerkey=c.customerkey and s.orderdate>='2016-01-01';
```

✅ **Good:**
```sql
SELECT 
    c.customerkey,
    c.givenname,
    c.surname,
    s.orderdate,
    s.quantity * s.netprice AS revenue
FROM sales s
JOIN customer c ON s.customerkey = c.customerkey
WHERE s.orderdate >= '2016-01-01';
```

---

## 🏆 Grading Criteria

Your submissions will be evaluated on:

### Technical Correctness (40%)
- ✅ Query produces correct results
- ✅ Handles edge cases properly
- ✅ Uses appropriate joins and aggregations
- ✅ Efficient approach

### Code Quality (20%)
- ✅ Clean, readable formatting
- ✅ Meaningful variable/alias names
- ✅ Proper comments
- ✅ Follows SQL best practices

### Business Understanding (20%)
- ✅ Answers the business question
- ✅ Provides actionable insights
- ✅ Shows critical thinking
- ✅ Recommendations are data-driven

### Documentation (20%)
- ✅ Clear explanation of approach
- ✅ Sample output included
- ✅ Assumptions noted
- ✅ Well-organized submission

---

## 💡 Tips for Success

1. **Start early** - Don't wait until the deadline
2. **Test thoroughly** - Use the [[Testing-Your-Queries|testing guide]]
3. **Seek feedback** - Ask classmates to review your work
4. **Revise and improve** - First draft is rarely the best
5. **Learn from examples** - Review sample solutions when provided
6. **Ask questions** - See [[Getting-Help|how to get help]]

---

← [[00-Index|Back to Home]]