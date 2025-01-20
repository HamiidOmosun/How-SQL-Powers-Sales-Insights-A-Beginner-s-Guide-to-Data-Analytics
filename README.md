# How SQL Powers Sales Insights: A Beginner’s Guide to Data Analytics

![conny-schneider-xuTJZ7uD7PI-unsplash](https://github.com/user-attachments/assets/4f98801b-c11d-4921-9668-522a0c339a17)

- [Introduction](#introduction)
- [Methodology](#methodology)
- [Findings](#findings)
- [Recommendations](#recommendations)
- [Conclusion](#conclusion)

## Introduction
In this post, I’ll share how I used SQL to analyze a sales dataset, extract actionable insights, and solve business challenges like identifying sales trends, optimizing pricing strategies, and understanding regional performance.

Our goal is to leverage the provided sales dataset to gain actionable insights into sales performance over time and predict future sales trends. We want to understand the factors driving sales across product categories, customer segments, and regions to make data-driven decisions for better inventory management, targeted marketing, and improved operational planning.

## Key Points We Are Trying to Achieve
### Sales Trends Analysis:
Identify how sales have evolved over time (e.g., monthly or yearly trends).
Recognize any patterns or seasonality that may impact sales performance.

### Product Performance:
Understand which product categories and sub-categories contribute most to revenue and profit.
Identify underperforming categories for potential optimization.

### Customer Segments:
Evaluate the sales contribution of different customer segments (e.g., Corporate, Consumer).
Analyze customer preferences to develop targeted sales strategies.

### Regional Insights:
Determine regional performance and identify high-performing and underperforming areas.
Use this information to allocate resources and tailor regional marketing efforts.

### Discount and Profitability:
Analyze the impact of discounts on sales and profitability.
Optimize pricing strategies to balance revenue growth and profitability.

## Dataset Description
Source: [This dataset was gotten from Kaggle, sample-superstore.csv ]

### Key Columns
- Order date.
- Region.
- Category
- Sub Category
- Quantity
- Sales
- Size: [9994 and 21 columns]

## Methodology
- Data Cleaning
- Analytical Approach
- Findings
- Recommendations
- Conclusion
  
### DATA CLEANING :
The first step I took was to create a new duplicate stagging table, rename the table and the columns to a better format for easy query.
then I went add to look for duplicates, null values, missing values and wrong formatting but the data set was very clean and well formatted the most import thing I did was to change the datatypes for columns like date
```sql
SELECT
STR_TO_DATE(Order_Date, '%m/%d/%Y') AS Order_Date
FROM sample_superstore_stagging;

UPDATE sample_superstore_stagging
SET Order_Date = STR_TO_DATE(Order_Date, '%m/%d/%Y');
```

## FINDINGS:
### Sales Trend:
Insight: Sales have increased over the last 4 years by 50%, with November having the highest sales due to the seasonality higher purchase in coming new year and festive periods.
```sql
SELECT
YEAR (Order_Date) AS Year_,
ROUND(SUM(Sales),2) AS Total_Sales
FROM sample_superstore_stagging
GROUP BY YEAR (Order_Date)
ORDER BY Year_;
```
![document 2](https://github.com/user-attachments/assets/7506279c-1e75-4b7d-8df6-11c741776be4)

```sql
SELECT
MONTH(Order_Date) AS Month,
ROUND(SUM(Sales),2) AS Total_Sales
FROM sample_superstore_stagging
GROUP BY MONTH(Order_Date)
ORDER BY Month ;
```
![eda 1](https://github.com/user-attachments/assets/dcdd5559-a8e3-499e-950c-a759baa15b8f)

### Product Performance:
Insight: The Category and Sub-Category that contributed most to sales was Technology and Phones as the Sub-Category bringing in a total revenue of $329753.09. And the Underperformers from both Category and Sub-Category were Office Supplies and Fasteners respectively, bringing in a total revenue of $3008.66 over the last 4 years.
```sql
SELECT
Category,
Sub_Category,
ROUND(SUM(Sales),2) AS Total_Revenue,
ROUND(SUM(Profit),2) AS Total_Profit
FROM
sample_superstore_stagging
GROUP BY
Category, Sub_Category
ORDER BY
Total_Revenue DESC;
```
![eda 2](https://github.com/user-attachments/assets/9f986db2-20c0-43c1-9bdb-39cb0cfc2a74)

### Customer Segments:
Insight: The consumer segment generated the most revenue and total sales

For the Consumer segment the top preferences were furniture — Chairs, Consumers generate high profit from Technology — smartphones

For the Cooperate segment the top preference were furniture — Chairs, Cooperate generate high profit from Technology — copiers

For the Home office segment the top preference were Technology — Phones, Home offices generate high profit from Technology — copiers
```sql
SELECT Segment,
ROUND(SUM(Sales),2) AS Total_Sales,
ROUND(SUM(Profit),2) AS Total_Revenue
FROM
sample_superstore_stagging
GROUP BY
Segment
ORDER BY
Total_Sales DESC;
```
![eda 3](https://github.com/user-attachments/assets/54d21e84-cbe3-457f-9369-d897a352245c)

### consumer segment
![eda 4](https://github.com/user-attachments/assets/9621a079-9d5d-419e-abbf-c5c4c8b26805)

### corporate segment
![eda 5](https://github.com/user-attachments/assets/3cbfa666-2bf9-4de9-8ed6-271a41dab087)

### home office segment
![eda 6](https://github.com/user-attachments/assets/ab99cb7a-896e-4d40-9509-f22debcab307)

## Regional Insights:
Insight: The west region accounted for 37.5% of the total profit, While the South region had the lowest performance
```sql
SELECT region,
ROUND(SUM(Sales),2) AS Total_Sales,
ROUND(SUM(Profit),2) AS Total_Profit
FROM
sample_superstore_stagging
GROUP BY
Region
ORDER BY
Total_Sales DESC;
```
![eda 7](https://github.com/user-attachments/assets/59743b06-0c12-4dd0-b404-217a8eead13f)

```sql
SELECT region,
Category,
ROUND(SUM(Sales),2) AS Total_Sales,
ROUND(SUM(Profit),2) AS Total_Profit
FROM
sample_superstore_stagging
GROUP BY
Region, Category, Sub_Category
ORDER BY
region, Total_Sales DESC;
```
![eda 8](https://github.com/user-attachments/assets/00bc22d9-0809-45bc-b22b-ceaddcf3fb22)

## Discount and Profitability:
Insight: 0% — 40% discount brought in the most sales , while 0% — 20% brought in the most profit and higher discount reduce profit margin put profits in negative.
```sql
SELECT discount,
ROUND(SUM(Sales),2) AS Total_Sales,
ROUND(SUM(Profit),2) AS Total_Profit
FROM
sample_superstore_stagging
GROUP BY discount
ORDER BY discount, Total_Sales DESC;
```
![eda 9](https://github.com/user-attachments/assets/5d0f6b79-6030-4e2c-95e7-eec7f96d5ae0)

```sql
SELECT
discount,
COUNT(*) AS total_transactions,
ROUND(SUM(sales),2) AS total_sales,
ROUND(SUM(profit),2) AS total_profit,
ROUND(AVG(sales),2) AS avg_sales,
ROUND(AVG(profit),2) AS avg_profit
FROM sample_superstore_stagging
GROUP BY discount
ORDER BY discount;
```
![eda 10](https://github.com/user-attachments/assets/da797168-7614-4677-baee-def996d78ab4)

## Recommendations:
Increase inventory from August to match the high sales starting in September ; the months with consistent sales and revenue was from march up until August and sales skyrocketed in months like September, November. To meet the demand spikes caused by seasonal trends.

Focus on Improving Product performance ; Expand the product range for phones and Technology While Increasing Marketing for Home office supplies .

Target the Profitable Customer segment; lunch a loyalty program for the consumer segment since they generated the most sales, while creating add campaigns for the home office segment.

Optimize regional strategy; Invest more in the West and East region while conducting market research for the central and south region to better understand their customer preference.

Refine discount strategy; Limit discounts to a maximum of 20% for most products to balance revenue and profit. Avoid blanket discounts and target specific customer segments or high-margin products.

## Conclusion:
This analysis highlights significant trends and actionable opportunities to improve sales performance, optimize inventory, and target key customer segments effectively. By focusing on the top-performing regions and refining discount strategies, the client can enhance profitability and sustain growth.

What do you think about this analysis? Let me know your thoughts in the comments, or feel free to connect with me for more insights on SQL and data analysis.





