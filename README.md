# Sales-Database-Analysis-MySQL-Project-

## 📌 Project Overview
This project is a **Sales Database Management and Analysis** system built in **MySQL**.  
It covers the full process of:
- Designing a relational database (customers, products, orders, order details)  
- Importing and managing sales data  
- Writing SQL queries to extract insights  

The project demonstrates **core SQL skills for data analysis**, including:
- Data modeling with relationships and foreign keys  
- Aggregations and grouping  
- Joins across multiple tables  
- Business-focused analytical queries  

---

## 🗂️ Database Schema
The database created is named **`sales`** and contains **4 main tables**:

### 1. customers
- `customerid` (PK)  
- `name`  
- `email`  
- `phone`  
- `city`  

### 2. products
- `productid` (PK)  
- `product_name`  
- `category`  
- `price`  
- `stock`  

### 3. orders
- `orderid` (PK)  
- `customerid` (FK → customers.customerid)  
- `order_date`  
- `total_amount`  

### 4. ordersdetails
- `orderdetail_id` (PK)  
- `orderid` (FK → orders.orderid)  
- `productid` (FK → products.productid)  
- `quantity`  
- `subtotal`

## 🔑 Key SQL Queries & Business Questions Answered
Some example analysis queries included in this project:

1. 📧 Find the email & phone number of **Alice Johnson**  
2. 🏙️ List all customers living in **Chicago**  
3. 📅 Display orders placed **after Feb 1, 2024**  
4. 👥 Count the **total number of customers**  
5. 📊 Show the **number of orders per customer**  
6. 🏷️ List each **product category** and count of products  
7. 💵 Calculate **total sales per customer**  
8. 💎 Find the **most expensive product**  
9. 📦 Retrieve all **orders with customer names**  
10. 🛒 Show products included in a specific order (`order_id = 1001`)  
11. 📦 Find the **total quantity and total price per product**  
12. 🔝 Get the **top 3 most frequently ordered products**  
13. 💰 Retrieve the **highest order total by each customer**  
14. 🥈 Find the **second most expensive product**  
15. 📆 Compute the **average monthly order total**  

---

## 📊 Insights & Learnings
From the analysis queries, we can answer questions like:
- **Who are the top customers by spending?**  
- **Which products are the most in-demand?**  
- **How do sales vary by month?**  
- **Which product categories contribute most to revenue?**


--- SCRIPTS

-- 1. CREATE THE DATABASE 
CREATE DATABASE sales;

-- 2. USING THE CREATED DATABASE 
USE sales;

-- 3. CREATING THE TABLES (customers, products, orders, ordersdetails)

CREATE TABLE customers (
                       customerid INT AUTO_INCREMENT,
                       name VARCHAR (35),
                       email TEXT,
                       phone TEXT,
                       city VARCHAR(50),
                       PRIMARY KEY (customerid)
                       );

CREATE TABLE products (
                       productid INT AUTO_INCREMENT,
                       product_name VARCHAR(40),
                       category VARCHAR(40),
                       price DECIMAL(10, 4),
                       stock INT,
                       PRIMARY KEY (productid)
                       );

CREATE TABLE orders (
                     orderid INT AUTO_INCREMENT,
                     customerid INT,
                     order_date TEXT,
                     total_amount DECIMAL(10, 4),
                     PRIMARY KEY (orderid),
                     FOREIGN KEY (customerid) REFERENCES customers(customerid)
                     );
                     
CREATE TABLE ordersdetails (
                            orderdetail_id INT AUTO_INCREMENT,
                            orderid INT,
                            productid INT,
                            quantity INT,
                            subtotal INT,
                            PRIMARY KEY (orderdetail_id),
                            FOREIGN KEY (orderid) REFERENCES orders(orderid),
                            FOREIGN KEY (productid) REFERENCES products(productid)
                            );

-- 4. USING THE DATA IMPORT WIZARD TO IMPORT RECORDS INTO EACH TABLE

-- 5. VIEWING EACH TABLE 
SELECT * FROM sales.customers;  
SELECT * FROM sales.orders;
SELECT * FROM sales.products;
SELECT * FROM sales.ordersdetails;

-- 6. Find the email and phone number of the customer named "Alice Johnson".
SELECT customers.email, customers.phone 
FROM customers
WHERE customers.name = "Alice Johnson" ;

-- 7. customers who lives in chicago
SELECT * FROM sales.customers
WHERE customers.city = "Chicago";

-- 8. Display all orders placed after 2024-02-01. 
SELECT * FROM sales.orders
WHERE orders.order_date > "2024-02-01";

-- 9. find the toal customers 
SELECT COUNT(*) AS 'Total Customers' FROM sales.customers;

-- 10. total number of orders each customer has places 
SELECT o.customerid,c.name, COUNT(*) AS 'total orders'
FROM orders o
INNER JOIN sales.customers c
ON o.customerid = c.customerid
GROUP BY o.customerid
ORDER BY COUNT(*) DESC;

-- 11. List each product category and the number of products in that category. 
SELECT products.category, COUNT(products.product_name) AS 'products count'
FROM sales.products
GROUP BY 1;

-- 12. Show the total sales amount per customer. 
SELECT c.customerid, c.name, ROUND(SUM(o.total_amount), 1) AS 'total sales'
FROM customers c
INNER JOIN orders o
ON c.customerid = o.customerid
GROUP BY 1, 2
ORDER BY 3 DESC;

-- 13. Find the most expensive product and its price. 
SELECT products.productid, products.product_name,
ROUND(products.price, 1) AS 'price'
FROM products
WHERE products.price = (SELECT MAX(products.price) FROM products);

-- 14. Retrieve all orders along with the corresponding customer names
SELECT o.orderid, o.customerid, c.name, 
o.order_date, o.total_amount
FROM sales.orders o
LEFT JOIN customers c
ON o.customerid = c.customerid
ORDER BY 2;

-- 15. Show which products were included in order_id = 1001. 
SELECT od.orderid, p.product_name
FROM sales.ordersdetails od
INNER JOIN sales.products p
ON od.productid = p.productid
WHERE od.orderid = '1001';

-- 16. Find the total quantity of each product sold and the total price.
SELECT p.productid, p.product_name, SUM(od.quantity) AS 'total quantity',
ROUND(SUM(p.price), 2) AS 'total price'
FROM products p
INNER JOIN sales.ordersdetails od
ON p.productid = od.productid
GROUP BY 1, 2
ORDER BY 3 DESC;

-- 17. Get the top 3 most frequently ordered products.
SELECT p.productid, p.product_name, SUM(od.quantity) AS 'total quantity'
FROM products p
INNER JOIN sales.ordersdetails od
ON p.productid = od.productid
GROUP BY 1, 2
ORDER BY 3 DESC
LIMIT 3;

-- 18. Retrieve the highest order total placed by each customer.
SELECT c.name, c.customerid, ROUND(MAX(o.total_amount), 1) AS 'highest amount'
FROM sales.orders o
INNER JOIN sales.customers c
ON o.customerid = c.customerid
GROUP BY 2, 1
ORDER BY 3 DESC;

-- 19. Show the second most expensive product.   
SELECT products.product_name, ROUND(products.price,1)
FROM sales.products
ORDER BY products.price DESC
LIMIT 1 OFFSET 1;

-- 20. Find the average order total per month. 
SELECT MONTH(orders.order_date) AS 'month', ROUND(AVG(orders.total_amount), 1)
AS 'average total'
FROM sales.orders 
GROUP BY 1;














