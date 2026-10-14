# DataCo Supply Chain Performance and Profitability Analytics

A complete data analytics project built in Python on the DataCo Smart Supply Chain dataset. It cleans and analyzes 180,519 order lines, answers twelve business questions about sales, products, customers, markets, shipping and profitability, and presents the results in an interactive five page Streamlit dashboard.

Built as a portfolio project for the KeDataLab Python for Data Analytics and Data Apps course.

Deployed dashboard: [add your Streamlit link here after deploying]

## Business Problem

DataCo operates across multiple markets, products, customers and shipping modes. The company needs to understand:

1. What drives sales.
2. Which products and markets are profitable.
3. How shipping performance varies.
4. Where shipment delays occur.
5. How customer segments perform.
6. Whether operational performance is associated with profitability.

## Main Project Question

How can DataCo's sales, customer, product and shipping data be analyzed to evaluate commercial performance, identify shipment delay patterns and understand their relationship with profitability?

The word relationship is used on purpose. The data is observational, so every finding in this project is an association. None of it is a claim that one factor causes another.

## Dataset

Source: the DataCo Smart Supply Chain for Big Data Analysis dataset on Kaggle.

1. DataCoSupplyChainDataset.csv is the transaction table with 180,519 rows and 53 columns. Orders run from January 2015 to January 2018.
2. DescriptionDataCoSupplyChain.csv is the data dictionary that explains what each of the 53 columns means.

What the data contains: 65,752 unique orders, 20,652 unique customers and 118 products across five markets and 164 order countries. Each row is one order line, not one whole order. That is why Average Order Value is calculated as total sales divided by unique orders, instead of a plain row average.

Other files that sometimes ship with this dataset, such as tokenized_access_logs.csv, are not used because nothing in the business problem needs them.

The data files are not stored in this repository because of their size. Download them from Kaggle and place them in data/raw/ as described in the reproduction steps below.

## Approach

The project follows one workflow from raw file to dashboard.

1. Load the raw CSV and understand every column.
2. Audit data quality: missing values, duplicates, key uniqueness and data types.
3. Remove unnecessary and personal information columns.
4. Fix data types and standardize column names.
5. Create derived variables such as shipping variance.
6. Define the KPIs mathematically before calculating them.
7. Answer twelve analytical questions with tables and charts.
8. Write insights and recommendations after every section.
9. Build the Streamlit dashboard on the cleaned data.

## Data Quality Findings and Cleaning Decisions

1. Product Description is 100 percent missing, so it was dropped.
2. Order Zipcode is about 86 percent missing, so it is not used. Geography is covered by order city, state, country, region and market.
3. There are no fully duplicated rows, and Order Item Id has no duplicates, so it is a reliable order line key.
4. Category Id is an exact duplicate of Product Category Id, so one copy was dropped.
5. Customer first name, last name, email, password and street were removed. They are personal information and are not needed for any business question.
6. Column names were standardized, for example Days for shipping (real) became actual_shipping_days.
7. The two date columns were converted to real datetime values.
8. Orders with a Shipping canceled status are excluded from every shipping metric, because a canceled order was never shipped on any schedule. This removes 7,754 order lines and leaves 172,765 eligible lines.
9. The original raw file is never modified. All cleaning is done on a copy.

## Derived Variables

Shipping variance is actual shipping days minus scheduled shipping days. A negative value means early, zero means on schedule and a positive value means late. Each order line is labeled Early, On Schedule or Late from this value. Discount rates are also grouped into bands: 0 percent, up to 10 percent, 10 to 20 percent, 20 to 30 percent and above 30 percent.

## KPIs

Values below are for the full dataset.

1. Total Sales: $36,784,735
2. Total Units Sold: 384,079
3. Total Order Items: 180,519
4. Average Order Value: $559.45 (total sales divided by unique orders)
5. Aggregate Order Profit: $3,966,903 (sum of the Benefit per order field)
6. Profit Margin: 10.78 percent
7. Late Shipment Rate: 57.29 percent (eligible orders only)
8. Average Shipping Variance: 0.57 days (eligible orders only)
9. On Schedule Rate: 18.64 percent (eligible orders only)
10. Average Discount Rate: 10.17 percent
11. Profit Per Unit: $10.33
12. Sales Per Customer: $1,781.17

The profit figure is called Aggregate Order Profit on purpose. It comes from the dataset's Benefit per order field, and the dataset does not confirm that it matches a conventional accounting profit.

## Analytical Questions and Key Findings

### Sales

1. How have sales and order volumes changed over time? September 2017 is the strongest sales month, at about $1.14 million.
2. Which categories and departments contribute the most sales and profit? Fishing leads the categories with about $6.9 million in sales, followed by Cleats and Camping and Hiking. Fan Shop is by far the largest department at about $17.1 million, followed by Apparel and Golf.
3. Which products sell the most? The Field and Stream Sportsman 16 Gun Fire Safe is the top product with about $6.9 million in sales.

Top 10 categories by total sales:

![Top 10 categories by total sales](https://github.com/sebmungai/DataCO-SuplyChain-Analysis/blob/main/Images/Top%2010%20Categories%20by%20Total%20Sales.png)

### Customers

4. How do customer segments differ? Consumer leads on sales at about $19.1 million, followed by Corporate at $11.2 million and Home Office at $6.5 million.
5. Which segment earns the most profit relative to sales? Consumer has the highest margin at 10.86 percent, but the three segments are very close (Corporate 10.77 percent, Home Office 10.59 percent). Margin does not separate the segments much.

Total sales by customer segment:

![Total sales by customer segment](https://github.com/sebmungai/DataCO-SuplyChain-Analysis/blob/main/Images/Total%20Sales%20by%20Customer%20Segment.png)


### Geography

6. Which markets and regions generate the most sales? Europe leads at about $10.9 million, with LATAM close behind at $10.3 million. Western Europe is the top region at about $5.9 million. USCA has the highest market margin at 11.14 percent, while Pacific Asia has the lowest at 10.37 percent.

Total sales by market:

![Total sales by market](https://github.com/sebmungai/DataCO-SuplyChain-Analysis/blob/main/Images/total_sales_by_market.png)

### Shipping

7. How does shipment performance differ by shipping mode? First Class has a 100 percent late rate, because every eligible First Class order is flagged late. Second Class is at 79.83 percent, Same Day at 47.93 percent and Standard Class at 39.77 percent.
8. Which regions have the highest delay rates? Central Africa is highest at 60.15 percent, followed by Western Europe at 58.53 percent.
9. Which categories have the highest delay rates? Golf Bags and Carts is highest at 68.85 percent, followed by Lacrosse at 62.61 percent and Pet Supplies at 61.44 percent.

Late shipment rate by shipping mode:

![Late shipment rate by shipping mode](https://github.com/sebmungai/DataCO-SuplyChain-Analysis/blob/main/Images/late_shipment_rate_by_mode.png)

### Profitability

10. How is discounting associated with profit? Average profit per order falls steadily as the discount band rises, from $26.67 with no discount to $23.39, $20.71 and $18.41 in the higher bands. This is an association and does not prove that discounting reduces profit.
11. Are shipment delays associated with profit? Only weakly. Average profit per order is $22.49 for early shipments, $22.71 for on schedule shipments and $21.62 for late shipments.
12. Which market, shipping mode and category combinations carry the most operational risk? Among combinations with at least 30 eligible orders, 85 of 375 are late every single time. Examples include USCA with First Class for Camping and Hiking and for Cardio Equipment.

## Recommendations

These follow from the findings and should be tested before acting on them.

1. Review the First Class service first. A 100 percent late rate is the largest single driver of the overall late rate.
2. Focus delay reviews on the highest risk regions and categories, such as Central Africa and Golf Bags and Carts, instead of applying one policy everywhere.
3. Cross check high delay categories against high sales categories. A category that is both large and late is a more urgent fix than a small one.
4. Review approval limits for deeper discounts, and run a controlled test before reducing discounts across the board.
5. Do not rely on shipping speed alone to improve per order profit, since its association with profit is weak in this data.


## Author

Sebastian Mungai

