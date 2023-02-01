# 1. List of dashboards for purchase

Below is a list of the dashbaord/report that are currently in use by the purchase and category management team. 

#### 1. [Purchase list](https://dub01.online.tableau.com/#/site/hblonlinesite/views/Purchase_list/product_list?:iid=3) and [total order value per sales class](https://dub01.online.tableau.com/#/site/hblonlinesite/views/Purchase_list/product_list?:iid=2) dashboard. 

The sql code used to generate the purchase list can be found [here](https://github.com/jahidrazan/Codes/blob/main/purchase_list.sql). The code is also used in the query editor in tableau. 


#### 2. [Purchase Overview Dashboard](https://dub01.online.tableau.com/#/site/hblonlinesite/views/purchase_overview_dashboard/PurchaseOverview?:iid=1) and [out of stock prodcuts](https://dub01.online.tableau.com/#/site/hblonlinesite/views/purchase_overview_dashboard/PurchaseOverview?:iid=1) dashboard. 

sql code that is used to show the out of stock trends per sales class can be found [here](https://github.com/jahidrazan/Codes/blob/main/Purchase%20Overview%20Dashboard_code_to_calculate_out_of_stock_percentage.sql). 

sql code for calculating stock magento and stock hbs can be found [here](https://github.com/jahidrazan/Codes/blob/main/calculate%20stock%20magento%20and%20stock%20hbs.sql)

sql code for calculating inbound, received, ordered and backorder value can be found [here](https://github.com/jahidrazan/Codes/blob/main/calculate%20inbound%2C%20ordered%20and%20backorder%20value.sql)
    
Please note that- **since we do not purchase the items having an average value lower than a certain threshold ( avg sales in last 42 days <0.05 which is applicable for C and D products consdiering yearly sales class ), the out of stock trend for those products are not shown in the dashboard**.
        
#### 3. [Last 3 months trend of stock Development per Sales Class](https://dub01.online.tableau.com/#/site/hblonlinesite/views/StockDevlopmentPerSalesClass/StockTrendDashboard?:iid=1) (Documentation Status: Complete)

The sql code for the dashboard can be found [here](https://github.com/jahidrazan/Codes/blob/main/stock_devlopment_per_sales_class.sql).

To see the historical trend of stock, the api_history table is used. FULL_ORDER_HISTORY table is used to show the [revenue comparison trend in the dashboard](https://dub01.online.tableau.com/#/site/hblonlinesite/views/StockDevlopmentPerSalesClass/RevenueandStockDevlopment?:iid=5). 

The dashboard uses all the fields from the query and there is no calculated field made in Tableau.

#### 4. [Weekly Stock Insights Bigquery Dashboard](https://lookerstudio.google.com/reporting/83a55b9f-dcd2-4342-b6c1-863cbc5021f8/page/tNy8B) (Documentation Status: Complete)

This is a data studio dashboard. Primarily used for weekly reporting of data via an excel file. Bram is the owner of the query. For access issue with this dashboard please open a gamil account with your fixami email-id and request access to Bram/Shivdutt.

#### 5. [Stock Health Per Root Category](https://dub01.online.tableau.com/#/site/hblonlinesite/views/Product_Stock_Inventory_with_Last28_Days_Sales/DashboardTotalValuePerRCANDSALESCLASS?:iid=1)

#### 6. [Shipping time per sales class](https://dub01.online.tableau.com/#/site/hblonlinesite/workbooks/1059466/views). 

The sql code can be found [here](https://github.com/jahidrazan/Codes/blob/main/shipping_time_and_revenue_per_sales_class.sql).

% A products shipped in 0-1 days : 



COUNT(if ([shipping_period_calc] =  '0-1 day shipping' AND [sales_class] = 'A') THEN   [product_id] END )

/ COUNT(if [sales_class]= 'A' THEN [product_id] END)

#### 7. [Overstock List](https://dub01.online.tableau.com/#/site/hblonlinesite/views/Overstockproductlistallproducts/OVERSTOCK_LIST?:iid=2)


#### 8. [CM Calcuation](https://dub01.online.tableau.com/#/site/hblonlinesite/views/CM_Calculation_including_Amazon/Cost_Dashboard?:iid=1) Dashboard. 

The sql code can be found [here](https://github.com/jahidrazan/Codes/blob/main/CM_calculation.sql). The sql code used in the query editor in tableau. 

# 2. Conceptual ERD
The Conceptual ERD is a good starting point to understand the relationship between products table and other associated information related to products:

   • Every product has a manufacturer and belong to a category (and root category).
   
   • Product can have one default supplier, no default supplier or multiple suppliers (no supplier is a default supplier).
   
   • Every product have multiple classifications.
   
   • Product may have have a child or may not. Some products are parent products and they have child items.
   
   • Products may have a level or 'new' or 'default'. 
   
   • Every product with a sellable stock has a product history (starting from Sep 2021). 
   
  It is important to note that the api.products table updated hourly during the day.

![](conceptual_ERD.PNG) 


  • Revenue and product tables are connected via product id. Product information missing in the revenue tables (such as product classification) can be added by joining the revenue tables with the products table. The easiest way to obtain the missing product information is often  by joining tables through the product tables via the key- which is product_id

  • All the business questions related to revenue, order, shipping method, and contribution margin can be answered based on the 3 tables. 
 
  • FULL HISTORY table contains all the sales history since 2018 and updated daily (3 am in the morning).
  
  • LM table keeps the data of the last month and updated hourly.
    
  • LTM table keeps the data of the last 12 months and updated daily (3 am in the morning).
  
  ![](product_and_revenue_tables_conceptual_ERD.PNG) 



# 2. DATA MODELS

Most problems related to product stock (and the purchase department) can be solved by joining the api tables.  

![](api_tables_ERD.PNG) 
  

 Also, some temporary tables has been created to solve the questions related to minimum order quantity, minimum desired quantity, super sales items and automatisch uitschakelen products.Please note that, these tables have been created using data from google sheet. A table created from the google sheet cannot be directly imported to tableau and therefore, a query has been made to see the output of the first table and the result has been saved as another bigquery table, with a name: _for_purchase and imported in the tableau analysis.
 
![](purchase_table_and_products_ERD.PNG) 



