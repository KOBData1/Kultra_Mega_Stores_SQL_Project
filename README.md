# Kultra_Mega_Stores_SQL_Project
This project analyzes sales and operations data from Kultra Mega Stores (KMS), a Nigerian retail and wholesale company specializing in office supplies and furniture. The focus is on uncovering actionable insights across product performance, customer segments, regional sales, and shipping efficiency using SQL.
Kultra Mega Stores (KMS) Inventory Analysis

## SQL Capstone Project – Incubator Hub
By: Karimot Olaitan Badru
## Company Overview
Kultra Mega Stores (KMS), headquartered in Lagos, Nigeria, is a leading provider of office supplies and furniture. It serves a diverse customer base including individuals, small businesses, and corporate clients. This project supports the Abuja division, analyzing order data between 2009 and 2012 to provide insights and recommendations.

## Tools Used
- SQL Server (T-SQL)
- Excel (for initial data inspection)
- GitHub (for documentation and version control)
## Datasets
- KMS — Order-level data containing customer info, sales, profit, and shipping details.
- Order_Status DSA — Order return status for matching customer records.

## Business Questions & SQL Analysis
### Case Scenario I
**1. Which product category had the highest sales?**
```sql
SELECT TOP 1 [Product_Category], COUNT([Product_Category]) AS [Product Count]
FROM [KMS]
GROUP BY Product_Category
ORDER BY [Product Count] DESC
```

**Insight:** The category with the most orders (and highest implied sales) is likely driving the majority of KMS’s revenue.

**2. What are the Top 3 and Bottom 3 regions in terms of sales?**
- Top 3
```sql
SELECT TOP 3 [Region], SUM([Sales]) AS [Total Sales]
FROM [KMS]
GROUP BY Region
ORDER BY [Total Sales] DESC
```

- Bottom 3 
```sql
SELECT TOP 3 [Region], SUM([Sales]) AS [Total Sales]
FROM [KMS]
GROUP BY Region
ORDER BY [Total Sales] ASC
```
**Insight:** Identifies strong and underperforming geographic markets.

**3. What were the total sales of appliances in Ontario?**
```sql
SELECT Region, SUM(Sales) AS [Total Sales]
FROM [KMS]
WHERE Region = 'Ontario'
GROUP BY Region
```

**Insight:** Focused product-region analysis for localized strategies.

**4. How can KMS increase revenue from the bottom 10 customers?**
```sql
SELECT TOP 10 [Customer_Name], SUM([Sales]) AS [Total Sales]
FROM [KMS]
GROUP BY Customer_Name
ORDER BY [Total Sales] ASC
```

**Insight:** Target these customers with personalized offers, follow-ups, or loyalty discounts.

**5. Which shipping method cost the most?**
```sql
SELECT TOP 1 [Ship_Mode], SUM([Shipping_Cost]) AS [Total Shipping Cost]
FROM [KMS]
GROUP BY Ship_Mode
ORDER BY [Total Shipping Cost] DESC
```

**Insight:** Helps in optimizing logistics cost.

### Case Scenario II
**6. Who are the most valuable customers and what do they typically purchase?**
```sql
SELECT [Customer_Name], Product_Name, SUM(Sales) AS [Total Sales]
FROM [KMS]
GROUP BY Customer_Name, Product_Name
ORDER BY [Total Sales] DESC
```

**Insight:** Highlights high-value customers and preferred product lines.

**7. Which small business customer had the highest sales?**
```sql
SELECT TOP 1 Customer_Name, Customer_Segment, SUM([Sales]) AS [Total Sales]
FROM [KMS]
WHERE Customer_Segment = 'Small Business'
GROUP BY Customer_Name, Customer_Segment
ORDER BY [Total Sales] DESC
```

**Insight:** Supports segmentation-based marketing or account management.

**8. Which corporate customer placed the most orders (2009–2012)?**
```sql
SELECT TOP 1 Customer_Name, Customer_Segment, COUNT([Order_ID]) AS [Total order]
FROM [KMS]
WHERE Customer_Segment = 'Corporate' AND Order_Date BETWEEN '2009' AND '2012'
GROUP BY Customer_Name, Customer_Segment
ORDER BY [Total order] DESC
```

**Insight:** Opportunity to deepen relationships with consistent corporate buyers.

**9. Which consumer customer was the most profitable?**
```sql
SELECT TOP 1 Customer_Name, Customer_Segment, SUM([Profit]) AS [Total profit]
FROM [KMS]
WHERE Customer_Segment = 'Consumer'
GROUP BY Customer_Name, Customer_Segment
ORDER BY [Total profit] DESC
```

**Insight:** High-margin customer segment to prioritize.

**10. Which customer returned items, and what segment do they belong to?**
```sql
SELECT Customer_Name, Customer_Segment, [Status]
FROM [KMS]
JOIN [Order_Status DSA]
ON [KMS].Order_ID = [Order_Status DSA].[Order_ID]
```

**Insight:** Returned orders may indicate dissatisfaction or delivery issues—requires customer service follow-up.

**11. Is shipping cost aligned with order priority?**
```sql
SELECT [Order_Priority], [Ship_Mode],
COUNT([Order_ID]) AS [Order Count],
SUM(Sales - Profit) AS [Estimated Shipping Cost],
AVG(DATEDIFF(DAY, [Order_Date], [Ship_Date])) AS [Avg ship date]
FROM [KMS]
GROUP BY Order_Priority, Ship_Mode
ORDER BY Order_Priority, Ship_Mode DESC
```

**Insight:** Evaluate if high-priority orders use Express Air and low-priority ones use Delivery Truck. Misalignment indicates waste.


## Key Insights & Recommendations
- Product Focus: Increase stock and promotions for best-selling product categories.
- Regions: Target low-sales regions with ads, partnerships, or discounts.
- Logistics Cost: Consider optimizing shipping methods—avoid using Express Air for low-priority orders.
- Customer Retention: Incentivize bottom-tier customers with loyalty rewards or bundled offers.
- Returns: Investigate patterns in returns to reduce product issues or delivery problems.

## Conclusion
This analysis helped uncover valuable insights across product categories, customer behavior, shipping efficiency, and regional performance. Using SQL, actionable intelligence can now drive data-informed decisions for the Abuja division of Kultra Mega Stores.

[View Full SQL Queries](https://drive.google.com/file/d/1rH7cW2w474EaXoM3EgyGnpO1RsqHwC5P/view?usp=drivesdk) 



