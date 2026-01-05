# Getting Help

## 🆘 Before Asking for Help

Try these steps first - you'll often solve the problem yourself!

### 1. Read the Error Message Carefully

Error messages usually tell you exactly what's wrong.

**Example Error:**
```
ERROR:  column "revenue" does not exist
LINE 5: ORDER BY revenue DESC
```

**What it means:** You're trying to ORDER BY a calculated column, but SQL doesn't see it because you calculated it with `SUM(quantity * netprice)` but didn't give it an alias.

**Fix:** Add `AS revenue` to your SELECT clause.

---

### 2. Check Your Syntax

Common syntax mistakes:

**Missing Comma:**
```sql
SELECT 
    productname
    categoryname  -- ❌ Missing comma!
FROM product;
```

**Wrong Quote Type:**
```sql
-- ❌ Wrong: Using backticks (works in MySQL, not PostgreSQL)
SELECT `productname` FROM `product`;

-- ✅ Correct: Use double quotes for identifiers
SELECT "productname" FROM product;

-- ✅ Better: Don't quote if not needed
SELECT productname FROM product;
```

**Missing Semicolon (in some tools):**
```sql
SELECT * FROM customer  -- May need semicolon
SELECT * FROM product;  -- ✅
```

---

### 3. Verify Table and Column Names

**Check spelling and case:**
```sql
-- ❌ Wrong table name
SELECT * FROM customers;  -- Table is 'customer' not 'customers'

-- ❌ Wrong column name
SELECT firstname FROM customer;  -- Column is 'givenname' not 'firstname'

-- ✅ Correct
SELECT givenname FROM customer;
```

**List all tables:**
```sql
SELECT table_name 
FROM information_schema.tables 
WHERE table_schema = 'public';
```

**List all columns in a table:**
```sql
SELECT column_name, data_type
FROM information_schema.columns
WHERE table_name = 'customer';
```

---

### 4. Test Parts Separately

Break your query into smaller pieces:

```sql
-- Start simple
SELECT * FROM sales LIMIT 5;

-- Add one join
SELECT * 
FROM sales s
JOIN product p ON s.productkey = p.productkey
LIMIT 5;

-- Add calculation
SELECT 
    s.quantity * s.netprice AS revenue
FROM sales s
JOIN product p ON s.productkey = p.productkey
LIMIT 5;

-- Add aggregation
SELECT 
    p.categoryname,
    SUM(s.quantity * s.netprice) AS revenue
FROM sales s
JOIN product p ON s.productkey = p.productkey
GROUP BY p.categoryname;
```

---

### 5. Google the Error

Most SQL errors have been solved before!

**Good search terms:**
- "PostgreSQL [your error message]"
- "SQL [what you're trying to do] example"
- "PostgreSQL GROUP BY error"

**Helpful sites:**
- Stack Overflow
- PostgreSQL documentation
- SQLZoo, Mode Analytics tutorials

---

## 📝 How to Ask a Good Question

### ❌ Bad Question

> "My query doesn't work. Help!"

**Problems:**
- No details about what's wrong
- No error message
- No query provided
- No explanation of what you expected

### ✅ Good Question

> "I'm trying to find the top 10 products by revenue for Problem 2.2, but my query returns duplicate rows. I expect 10 rows but I'm getting 20.
>
> Here's my query:
> ```sql
> SELECT 
>     p.productname,
>     SUM(s.quantity * s.netprice) AS revenue
> FROM sales s
> JOIN product p ON s.productkey = p.productkey
> GROUP BY p.productname
> ORDER BY revenue DESC
> LIMIT 10;
> ```
>
> I think the issue might be in my GROUP BY, but I'm not sure what's wrong. Should I be grouping by productkey instead of productname?"

**Why this is good:**
- ✅ Specific problem statement
- ✅ Query provided
- ✅ Expected vs actual results
- ✅ Your hypothesis about the issue
- ✅ Clear, concise

---

## 🎯 Question Template

Use this template when asking for help:

```markdown
## What I'm Trying to Do
[Describe the business problem you're solving]

## What's Happening
[Describe the issue - error message, wrong results, etc.]

## What I Expected
[What results did you expect?]

## My Query
[paste your SQL query]

## What I've Tried
[List the things you've already attempted]

## My Hypothesis
[What do you think might be causing the problem?]
```

---

## 💬 Example Questions

### Example 1: Error Message

```markdown
## What I'm Trying to Do
Calculate monthly revenue for Problem 4.1

## What's Happening
Getting error: "column reference 'month' is ambiguous"

## My Query
SELECT 
    DATE_TRUNC('month', orderdate) AS month,
    SUM(quantity * netprice) AS revenue
FROM sales
JOIN date ON sales.datekey = date.datekey
GROUP BY month
ORDER BY month;

## What I've Tried
- Checked spelling of 'month'
- Made sure date is properly formatted

## My Hypothesis
Both sales and date tables might have a 'month' column, causing confusion?
```

**Answer:** Yes! Use table aliases or qualify the column:
```sql
GROUP BY DATE_TRUNC('month', sales.orderdate)
-- or just: GROUP BY 1
```

---

### Example 2: Wrong Results

```markdown
## What I'm Trying to Do
Find customers with more than 5 orders (Problem 3.1)

## What's Happening
My query returns 0 rows, but I know there should be many customers with 5+ orders.

## What I Expected
Should return hundreds or thousands of customers

## My Query
SELECT 
    c.customerkey,
    COUNT(s.saleskey) AS order_count
FROM customer c
LEFT JOIN sales s ON c.customerkey = s.customerkey
WHERE COUNT(s.saleskey) > 5
GROUP BY c.customerkey;

## What I've Tried
- Changed > 5 to > 1 (still returns 0)
- Removed the WHERE clause (works, returns all customers)

## My Hypothesis
Can't use COUNT() in WHERE clause?
```

**Answer:** Correct! Use HAVING instead of WHERE for aggregated conditions:
```sql
HAVING COUNT(DISTINCT s.orderkey) > 5
-- Also note: COUNT orders, not sales rows!
```

---

### Example 3: Performance Issue

```markdown
## What I'm Trying to Do
Calculate revenue by product category (Problem 2.1)

## What's Happening
Query has been running for 10 minutes and hasn't finished

## My Query
SELECT 
    p.categoryname,
    SUM(s.quantity * s.netprice) AS revenue
FROM sales s
CROSS JOIN product p
WHERE s.productkey = p.productkey
GROUP BY p.categoryname;

## What I've Tried
- Added LIMIT 10 (still slow)
- Removed ORDER BY (still slow)

## My Hypothesis
Maybe the sales table is just really big?
```

**Answer:** You're using CROSS JOIN instead of proper JOIN! This creates a Cartesian product (20M × 2,500 = 50 BILLION rows!)
```sql
-- Fix: Use proper JOIN
FROM sales s
JOIN product p ON s.productkey = p.productkey
```

---

## 🔍 Troubleshooting Flowchart

```
Is there an error message?
│
├─ YES → Read it carefully
│         ├─ Syntax error? → Check commas, quotes, parentheses
│         ├─ Column doesn't exist? → Check spelling, table name
│         └─ Permission denied? → Check with instructor
│
└─ NO → Query runs but results are wrong?
          ├─ Too many rows? → Check your JOINs (Cartesian product?)
          ├─ Too few rows? → Check WHERE/HAVING filters
          ├─ Wrong numbers? → Verify calculations, check for NULLs
          └─ Weird data? → Check data types, look at raw data
```

---

## 📚 Resources

### Official Documentation
- [PostgreSQL Documentation](https://www.postgresql.org/docs/)
- [PostgreSQL Tutorial](https://www.postgresqltutorial.com/)

### Interactive Learning
- [SQLZoo](https://sqlzoo.net/)
- [Mode Analytics SQL Tutorial](https://mode.com/sql-tutorial/)
- [LeetCode SQL Problems](https://leetcode.com/problemset/database/)

### Quick References
- [[Appendix-Database-Schema|Database Schema]]
- [[Tips-For-Success|Tips for Success]]
- [[Testing-Your-Queries|Testing Your Queries]]

### Video Tutorials
- Search YouTube for "PostgreSQL [topic]"
- Look for channels like Fireship, Programming with Mosh, freeCodeCamp

---

## 👥 Where to Ask

### During Office Hours
Best option for complex problems requiring back-and-forth discussion.

**Prepare:**
- Have your query ready
- Show the error message
- Explain what you've tried
- Be ready to screen share

### On Discussion Forum/Slack
Good for questions that might help other students too.

**Format your code:**

Use code blocks for SQL:
```sql
SELECT * FROM sales LIMIT 5;
```

### Study Groups
Explaining problems to peers often helps you solve them yourself!

**But remember:**
- Collaboration is encouraged
- Copying is not
- You must understand your own solution

---

## 🚫 What NOT to Do

### Don't Ask Others to Do Your Work

❌ "Can someone write the query for Problem 3.2 for me?"

✅ "I'm stuck on Problem 3.2. I've calculated recency and frequency, but I'm not sure how to combine them into segments. Should I use nested CASE statements?"

### Don't Share Complete Solutions Publicly

❌ Posting your entire working solution in a public channel

✅ Asking about a specific part you're stuck on, or helping others understand concepts without giving away answers

### Don't Wait Until the Last Minute

❌ "Assignment due in 2 hours, query not working, help!"

✅ Start early, ask questions as they come up

---

## 🎓 Learn From Your Mistakes

Keep a "lessons learned" document:

```markdown
## Mistake: Used COUNT(orderkey) instead of COUNT(DISTINCT orderkey)

**Problem:** Got wrong number of orders per customer

**What I learned:** Multiple sales rows can belong to same order

**Fix:** Always use COUNT(DISTINCT orderkey) for counting orders

**Date:** 2024-01-15
```

This helps you avoid repeating mistakes!

---

## 💡 Remember

> The best way to learn SQL is by struggling through problems and figuring them out. Don't be discouraged by errors - they're part of the learning process!

**Key principles:**
- 🔍 **Debug systematically** - Don't just try random changes
- 📖 **Read documentation** - It's there for a reason
- 🤔 **Think critically** - Why might this be happening?
- 👥 **Collaborate (appropriately)** - Learn from each other
- 🧘 **Be patient** - Complex queries take time to get right

---

← [[00-Index|Back to Home]]