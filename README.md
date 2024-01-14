/*1. Which product categories drive the biggest profits? Is this the same/across store
locations?
Finding Product Categories with the Biggest Profits;*/
SELECT
    "Product_Category",
    SUM("Product_Price" - "Product_Cost") AS total_profits
FROM
    products
GROUP BY
    "Product_Category"
ORDER BY
    total_profits DESC;
/*Then Profits by Product Category and Store Location*/
SELECT
    s."Store_Location",
    p."Product_Category",
    SUM(p."Product_Price" - p."Product_Cost") AS category_profits
FROM
    sales s
INNER JOIN products p ON s."Product_ID" = p."Product_ID"
GROUP BY
    s."Store_Location", p."Product_Category"
ORDER BY
    s."Store_Location", category_profits DESC;

/*2. How much money is tied up in inventory at the toy stores? How long will it last?*/
SELECT
    SUM("Product_Cost" * "Inventory_Level") AS total_inventory_value
FROM
    products;

SELECT
    SUM("Product_Cost" * "Inventory_Level") AS total_inventory_value,
    SUM("Inventory_Level") AS total_inventory_units,
    AVG("Daily_Sales") AS average_daily_sales
FROM
    products;

/*3. Are sales being lost with out-of-stock products at certain locations?*/ 
SELECT
    s."Store_Location",
    COUNT(*) AS out_of_stock_products
FROM
    sales s
LEFT JOIN products p ON s."Product_ID" = p."Product_ID"
WHERE
    p."Inventory_Level" = 0
GROUP BY
    s."Store_Location";
