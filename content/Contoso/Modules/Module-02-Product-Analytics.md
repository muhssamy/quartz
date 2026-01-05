# Module 2: Product Analytics

> [!quote] Business Context
> **From**: Product Management Team  
> **To**: Analytics Team  
> **Need**: We need to understand which product categories and individual products are driving our business. This helps us decide where to invest in inventory, marketing, and shelf space.

---

## Problem 2.1: Category Performance Ranking

> [!question] The Business Question
> **"Which product categories should we invest in?"**
> 
> We need to rank our categories by performance to guide investment decisions.

**📋 What You Need to Deliver:**

One row per **product category** showing:

- Category Name
- Total Revenue
- Total Units Sold
- Number of Distinct Products in category
- Average Price Point (of products in category)
- Profit Margin %

Sort from highest to lowest revenue.

> [!tip] You'll Need To
> - Join `sales` and `product` tables
> - Group by category
> - Calculate multiple aggregations per group
> - Think about what makes a category "good" - is it just revenue?

> [!example] Think About This
> A category with high revenue but low margin might not be as good as one with lower revenue but higher margin. The business needs both numbers!

---

## Problem 2.2: Top 10 Best-Selling Products

> [!question] The Business Question
> **"Which specific products should we promote in our marketing campaigns?"**
> 
> Marketing wants to know our star products to feature in ads.

**📋 What You Need to Deliver:**

Show the **top 10 products** by total unit volume:

- Product Name
- Category Name
- Brand
- Total Units Sold
- Total Revenue
- Number of Different Orders containing this product

> [!tip] Breaking It Down
> - Join sales to products to get product details
> - Group by product (and include category/brand)
> - Sort by units sold
> - Limit to top 10
> - Don't confuse order count with number of sales rows!

---

## Problem 2.3: Product Portfolio Matrix

> [!question] The Business Question
> **"Which products are 'stars' vs 'dogs' in our portfolio?"**
> 
> We need to classify products to decide which to promote, maintain, or discontinue.

**📋 What You Need to Deliver:**

Categorize products into quadrants:

- **High Revenue** (>$100,000) vs **Low Revenue** (≤$100,000)
- **High Margin** (>50%) vs **Low Margin** (≤50%)

Show how many products fall into each category:

- Star: High Revenue + High Margin
- Cash Cow: High Revenue + Low Margin
- Question Mark: Low Revenue + High Margin
- Dog: Low Revenue + Low Margin

> [!note] Business Strategy by Quadrant
> - **Stars**: Invest and promote heavily
> - **Cash Cows**: Maintain, they fund the business
> - **Question Marks**: Could be stars with right investment
> - **Dogs**: Consider discontinuing

> [!tip] Think About
> - You'll need CASE statements to classify products
> - First calculate revenue and margin per product
> - Then categorize based on the thresholds
> - Finally, count how many in each category

---

## Problem 2.4: Slow-Moving Inventory Analysis

> [!question] The Business Question
> **"Which products should we discount or discontinue?"**
> 
> Inventory costs money - we need to identify products that aren't selling.

**📋 What You Need to Deliver:**

Find products that have:

- Less than 10 total units sold ever, OR
- No sales in the last 3 months ==filter last quarter out==

For each product show:

- Product Name
- Last Sale Date (when it last sold)
- Total Units Ever Sold
- Days Since Last Sale

> [!tip] Approach
> - Group sales by product
> - Calculate total units and last sale date
> - Filter to products meeting the criteria
> - Calculate days between last sale and today
> - Think: Some products might have ZERO sales - how do you handle those?

> [!warning] Edge Cases
> What about brand new products with no sales yet? Should they appear in this list?

---

## 💡 Module Summary

In this module, you learned to:
- Join multiple tables (sales + products)
- Group by product attributes
- Calculate product-level metrics
- Use CASE statements for classification
- Work with date calculations
- Identify business insights from data patterns

---

← [[Module-01-Executive-Dashboard|← Previous: Module 1]] | [[00-Index|Home]] | [[Module-03-Customer-Analytics|Next: Module 3 →]]