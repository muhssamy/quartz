# Module 11: Advanced Business Logic

> [!quote] Business Context
> **From**: Advanced Analytics Team  
> **To**: Senior Analysts  
> **Need**: We need sophisticated analysis that requires complex business logic. These insights drive major strategic decisions.

---

## Problem 11.1: Product Affinity Analysis (Market Basket)

> [!question] The Business Question
> **"Which products are frequently bought together?"**
> 
> This powers "Customers who bought X also bought Y" recommendations.

**📋 What You Need to Deliver:**

Find product pairs that appear in the same order:

- Product A Name
- Product B Name
- Number of Orders Containing Both
- What % of customers who bought A also bought B

> [!example] Business Value
> If 60% of customers who buy cameras also buy memory cards:
> - Bundle them together
> - Put memory cards near cameras in store
> - Recommend memory cards when camera is added to cart online

> [!warning] Challenge
> You need to find which products appear TOGETHER in orders. This requires matching products within the same order - tricky!

---

## Problem 11.2: Customer Churn Risk Scoring

> [!question] The Business Question
> **"Which customers are at risk of churning (leaving us)?"**
> 
> Identifying at-risk customers early lets us save them.

**📋 What You Need to Deliver:**

Find customers who:

- Haven't purchased in 90+ days (going dormant)
- Previously were active (had 5+ orders)
- Their order frequency is declining
- Include their last purchase details

> [!note] Churn Indicators
> - **High Risk**: Was buying monthly, now 6 months since last purchase
> - **Medium Risk**: Was buying quarterly, now 4 months since last
> - **Low Risk**: Regular purchases continuing

> [!tip] What Marketing Does With This
> - Win-back email campaigns
> - Special discount offers
> - "We miss you" outreach
> - Exit surveys to understand why

---

## Problem 11.3: Price Elasticity Analysis

> [!question] The Business Question
> **"How do price changes affect sales volume?"**
> 
> Should we raise prices? Lower them? This analysis helps decide.

**📋 What You Need to Deliver:**

Find products where price changed, then compare:

- Product Name
- Date of Price Change
- Old Price
- New Price
- Average Daily Volume Before Change (30 days prior)
- Average Daily Volume After Change (30 days after)
- % Change in Volume

> [!note] Price Elasticity Concept
> - **Elastic**: Volume drops a lot when price increases (customers are price-sensitive)
> - **Inelastic**: Volume stays steady despite price changes (customers will pay)
> - Luxury items often inelastic
> - Commodities often elastic

---

## Problem 11.4: ABC Inventory Classification

> [!question] The Business Question
> **"How should we prioritize inventory management?"**
> 
> Not all products deserve equal attention - focus on what matters.

**📋 What You Need to Deliver:**

Classify every product:

- **A items**: Top 20% of products contributing ~80% of revenue (manage closely!)
- **B items**: Next 30% of products contributing ~15% of revenue (moderate attention)
- **C items**: Remaining 50% contributing ~5% of revenue (minimal management)

> [!example] Management Strategy
> - **A items**: Daily monitoring, never out of stock, prime shelf space
> - **B items**: Weekly review, acceptable to occasionally back-order
> - **C items**: Monthly review, can discontinue slow movers

---

## 💡 Module Summary

In this module, you learned to:
- Perform market basket analysis
- Calculate churn risk indicators
- Analyze price elasticity
- Implement ABC classification
- Apply cumulative percentage calculations
- Translate complex business logic into SQL

---

← [[Module-10-Data-Quality|← Previous: Module 10]] | [[00-Index|Home]] | [[Module-12-Expert-Challenges|Next: Module 12 →]]