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




