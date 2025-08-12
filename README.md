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

  <img width="1000" height="400" alt="image" src="https://github.com/user-attachments/assets/9f048ae9-db36-4a26-a767-26b3fe220a4e" />
  

  <img width="400" height="400" alt="image" src="https://github.com/user-attachments/assets/8efe437d-ca2d-4a34-9101-a9df0e3a343d" />




## Business Questions & SQL Analysis
### Case Scenario I
**1. Which product category had the highest sales?**
```sql
SELECT TOP 1 [Product_Category], COUNT([Product_Category]) AS [Product Count]
FROM [KMS]
GROUP BY Product_Category
ORDER BY [Product Count] DESC
```
**Answer:** Office supplies – 4,589
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
**Answer:**
**Top 3:** West – 3,547,960; Ontario – 3,019,152; Prairie – 2,773,568
**Bottom 3:** Nunavut – 1,062,091; Northwest Territories – 766,755; Yukon – 971,446
**Insight:** Identifies strong and underperforming geographic markets.

**3. What were the total sales of appliances in Ontario?**
```sql
SELECT SUM ([Sales]) AS [Total Sales of Appliances in Ontario]
FROM [KMS]
WHERE [Product_Category] ='appliances' 
AND [Region] = 'Ontario'
```
**Answer:** There was no sale of appliances in Ontario.
**Insight:** Focused product-region analysis for localized strategies.

**4. How can KMS increase revenue from the bottom 10 customers?**
```sql
SELECT TOP 10 [Customer_Name], SUM([Sales]) AS [Total Sales]
FROM [KMS]
GROUP BY Customer_Name
ORDER BY [Total Sales] ASC
```
**Insight:** Target these customers with personalized offers, follow-ups, or loyalty discounts.

<img width="900" height="300" alt="image" src="https://github.com/user-attachments/assets/9e195f6d-94c6-49c5-a41d-652107706edd" />







**5. KMS incurred the most shipping cost using which shipping method?**
```sql
SELECT TOP 1 [Ship_Mode], SUM([Shipping_Cost]) AS [Total Shipping Cost]
FROM [KMS]
GROUP BY Ship_Mode
ORDER BY [Total Shipping Cost] DESC
```
**Answer:** They incurred the most delivery cost using the Delivery Truck shipping method.
**Insight:** Helps in optimizing logistics cost.

### Case Scenario II
**6. Who are the most valuable customers and what do they typically purchase?**
```sql
SELECT [Customer_Name], Product_Name, SUM(Sales) AS [Total Sales]
FROM [KMS]
GROUP BY Customer_Name, Product_Name
ORDER BY [Total Sales] DESC
```
**Answer:** Emily Phan, Jasper Cacioppo, Craig Carreira — all purchased Polycom Viewstation.
**Insight:** Highlights high-value customers and preferred product lines.

**7. Which small business customer had the highest sales?**
```sql
SELECT TOP 1 Customer_Name, Customer_Segment, SUM([Sales]) AS [Total Sales]
FROM [KMS]
WHERE Customer_Segment = 'Small Business'
GROUP BY Customer_Name, Customer_Segment
ORDER BY [Total Sales] DESC
```
**Answer:** Dennis Kane
**Insight:** Supports segmentation-based marketing or account management.

**8. Which corporate customer placed the most orders (2009–2012)?**
```sql
SELECT TOP 1 Customer_Name, Customer_Segment, COUNT([Order_ID]) AS [Total order]
FROM [KMS]
WHERE Customer_Segment = 'Corporate' AND Order_Date BETWEEN '2009' AND '2012'
GROUP BY Customer_Name, Customer_Segment
ORDER BY [Total order] DESC
```
**Answer:** John Lee
**Insight:** Opportunity to deepen relationships with consistent corporate buyers.

<img width="900" height="300" alt="image" src="https://github.com/user-attachments/assets/5c06b546-eba5-4273-b9a4-6463af23e7d0" />







**9. Which consumer customer was the most profitable?**
```sql
SELECT TOP 1 Customer_Name, Customer_Segment, SUM([Profit]) AS [Total profit]
FROM [KMS]
WHERE Customer_Segment = 'Consumer'
GROUP BY Customer_Name, Customer_Segment
ORDER BY [Total profit] DESC
```
**Answer:** Emily Phan
**Insight:** High-margin customer segment to prioritize.

**10. Which customer returned items, and what segment do they belong to?**
```sql
SELECT DISTINCT 
    KMS.Customer_Name, 
    KMS.Customer_Segment, 
    o.Status
FROM KMS
JOIN [Order_Status DSA] AS o
    ON KMS.Order_ID = o.Order_ID
WHERE o.Status = 'Returned';
```
**Answer:** 417 customers returned orders across the different segments.
**Insight:** Returned orders may indicate dissatisfaction or delivery issues—requires customer service follow-up.

<img width="800" height="300" alt="image" src="https://github.com/user-attachments/assets/67583b8a-4574-4b0e-8255-eb41134aff0b" />


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
**Answer:** No. Some low/medium-priority orders were sent via Regular/Express Air, and some high/critical-priority orders were sent via Delivery Truck, which is slow. Orders were not shipped based on priority — Express Air should be used for urgent orders.
**Insight:** Evaluate if high-priority orders use Express Air and low-priority ones use Delivery Truck. Misalignment indicates waste.
<img width="800" height="300" alt="image" src="https://github.com/user-attachments/assets/c8258131-f9fc-4649-be20-5f35a77da28e" />


<img width="10005" height="300" alt="image" src="https://github.com/user-attachments/assets/3d49eb84-8ee7-4f0a-a5cf-e6ba9206fc4a" />







## ✅ Recommendations

**1. Boost High-Performing Categories**
   - Expand Office Supplies with more product variety, bundle offers, and loyalty perks.
   - Promote cross-sells with related categories like Electronics.

**2. Improve Sales in Underperforming Regions**
   - Launch targeted marketing in Nunavut, Yukon, and Northwest Territories.
   - Offer first-time buyer incentives and assess logistics challenges.

**3. Address Product Gaps in Regions**
   - Investigate why there were no Appliance sales in Ontario.
   - Test local demand with pilot campaigns and review stocking issues.

**4. Re-engage Bottom 10 Customers**
   - Analyze their purchase history and send personalized offers.
   - Introduce loyalty or referral incentives to revive engagement.

**5. Align Shipping Method with Order Priority**
   - Set clear rules for assigning shipping methods based on order urgency.
   - Automate shipping selection and train fulfillment staff.

**6. Retain High-Value Customers**
   - Offer VIP programs, referral bonuses, and early access to new products.
   - Collect feedback regularly to keep satisfaction high.

**7. Monitor & Reduce Product Returns**
  - (Once data is complete) Review returns by segment or product.
  - Improve descriptions, packaging, and customer communication to reduce return rates.

## Conclusion
This analysis helped uncover valuable insights across product categories, customer behavior, shipping efficiency, and regional performance. Using SQL, actionable intelligence can now drive data-informed decisions for the Abuja division of Kultra Mega Stores.

[View Full SQL Queries](https://drive.google.com/file/d/1rH7cW2w474EaXoM3EgyGnpO1RsqHwC5P/view?usp=drivesdk) 



