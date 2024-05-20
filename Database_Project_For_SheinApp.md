<h1>Database Project for SheinApp </h1>

The scope of this project is to use all the SQL knowledge gained throught the Software Testing course and apply them in practice.

Application under test: **SheinApp**

Tools used: MySQL Workbench

Database description: **The database supports SheinApp, an e-commerce platform for fashion products. It stores information about products, categories, customers, orders, and order details. This facilitates inventory management, customer accounts, order processing, and order details.**

<ol>
<li>Database Schema </li>
<br>
You can find below the database schema that was generated through Reverse Engineer and which contains all the tables and the relationships between them.

![image](https://github.com/ElenaPdu/ITFactory_MySQL_Sample/assets/167423014/10020840-44cc-40d3-92cf-5a7a8dde8456)


The tables are connected in the following way:

<ul>
  <li> The "Categories" table is connected with the "Products" table through a one-to-many relationship, where the primary key is "category_id" in the "Categories" table, and the foreign key is "category_id" in the "Products" table.
 </li> 
  <li> The "Products" table is connected with the "OrderDetails" table through a one-to-many relationship, where the primary key is "product_id" in the "Products" table, and the foreign key is "product_id" in the "OrderDetails" table.
</li>
  <li> The "Orders" table is connected with the "OrderDetails" table through a one-to-many relationship, where the primary key is "order_id" in the "Orders" table, and the foreign key is "order_id" in the "OrderDetails" table.
</li>
  <li> The "OrderDetails" table is connected with the "Products" table through a many-to-one relationship, where the primary key is "product_id" in the "Products" table, and the foreign key is "product_id" in the "OrderDetails" table.
</li>
  <li> The "Customers" table is connected with the "Orders" table through a one-to-many relationship, where the primary key is "customer_id" in the "Customers" table, and the foreign key is "customer_id" in the "Orders" table.
</li>
</ul><br>

<li>Database Queries</li><br>

<ol type="a">
  <li>DDL (Data Definition Language)</li> <br>

  The following instructions were written in the scope of CREATING the structure of the database (CREATE INSTRUCTIONS)

-- Create a new database named SheinApp. <br>
**CREATE DATABASE SheinApp;** <br>
  
-- Create the "Categories" table with the columns "category_id", "category_name" and "description" used for storing the categories of products available in the application. <br>
**CREATE TABLE Categories (category_id INT AUTO_INCREMENT PRIMARY KEY, <br>
category_name VARCHAR(255) NOT NULL, description TEXT);** <br>
  

-- Create the "Products" table with the columns "product_id", "product_name", "category_id", "price" and "stock_quantity".
"category_id" is a foreign key referring to "category_id" in the "Categories" table.<br>
**CREATE TABLE Products (product_id INT AUTO_INCREMENT PRIMARY KEY, product_name VARCHAR(255) NOT NULL, category_id INT, price DECIMAL(10, 2), FOREIGN KEY (category_id) REFERENCES Categories(category_id));** <br>

-- Create the "Customers" table to store customer information, including "customer_id", "first_name", "last_name" and "email". <br>
**CREATE TABLE Customers (customer_id INT AUTO_INCREMENT PRIMARY KEY, <br>
first_name VARCHAR(50), last_name VARCHAR(50), email VARCHAR(100));** <br>


-- Create the "Orders" table to manage customer orders, including "order_id", "customer_id", "order_date" and "total_amount". <br>
**CREATE TABLE Orders (order_id INT AUTO_INCREMENT PRIMARY KEY, customer_id INT, order_date DATE, total_amount DECIMAL(10, 2), FOREIGN KEY (customer_id) REFERENCES Customers(customer_id));** <br>

-- Create the "OrderDetails" table to track order details, including "order_detail_id", "order_id", "product_id", "quantity" and "price."
"order_id" and "product_id" are foreign keys referring to "order_id" and "product_id" in the "Orders" and "Products" tables. <br>
**CREATE TABLE OrderDetails (order_detail_id INT AUTO_INCREMENT PRIMARY KEY, order_id INT, product_id INT, quantity INT, price DECIMAL(10, 2), FOREIGN KEY (order_id) REFERENCES Orders(order_id), FOREIGN KEY (product_id) REFERENCES Products(product_id));** <br>

  After the database and the tables have been created, a few ALTER instructions were written in order to update the structure of the database, as described below:

-- Add a new column "stock_quantity" to the "Products" table to track the available stock for each product. <br>
**ALTER TABLE Products <br>
ADD COLUMN stock_quantity INT AFTER price;** <br>

-- Add a new column "category_code" to the "Categories" table to store a unique code for each category. <br>
**ALTER TABLE Categories <br>
ADD COLUMN category_code VARCHAR(10) AFTER description;** <br>

-- Rename the column "stock_quantity" to "available_quantity" to make the schema more explicit. <br>
**ALTER TABLE Products <br>
CHANGE COLUMN stock_quantity available_quantity INT;** <br>

-- Add a new column "order_status" to the "Orders" table with a default value of "pending" to track the status of orders. <br>
**ALTER TABLE Orders <br>
ADD COLUMN order_status VARCHAR(20) DEFAULT 'pending';** <br>

-- Modify the length of the "email" column from "VARCHAR(100)" to "VARCHAR(255)" to allow longer email addresses. <br>
**ALTER TABLE Customers <br>
MODIFY COLUMN email VARCHAR(255);** <br>
 
  
  <li>DML (Data Manipulation Language)</li> <br>

  In order to be able to use the database I populated the tables with various data necessary in order to perform queries and manipulate the data. 
  In the testing process, this necessary data is identified in the Test Design phase and created in the Test Implementation phase. 

  Below you can find all the insert instructions that were created in the scope of this project:

-- Insert data into the "Categories" table. The involved columns are "category_name" and "description". 
Three rows are added to the "Categories" table, each representing a different category of products: "Electronics" for electronic devices and accessories, "Clothing" for men's, women's and children's clothing items, 
and "Home Decor" for decorative objects for the interior design of the house. <br>
**INSERT INTO Categories (category_name, description) <br>
VALUES <br>
('Electronics', 'Products related to electronic devices and accessories'), <br>
('Clothing', 'Clothing items for men, women, and children'), <br>
('Home Decor', 'Decorative items for home interior design');** <br>

-- Insert data into the "Products" table. The involved columns are "product_name", "category_id", "price" and "available_quantity." 
Three products are added to the "Products" table, each associated with a specific category through the "category_id" column. <br>
**INSERT INTO Products (product_name, category_id, price, available_quantity) <br>
VALUES <br>
('Smartphone', 1, 799.99, 100), <br>
('T-shirt', 2, 19.99, 250), <br>
('Table Lamp', 3, 49.99, 50), <br>
('Keyboard', 1, 25.99, 55), <br>
('Laptop', 1, 1299.99, 50), <br>
('Jeans', 2, 39.99, 150), <br>
('Desk Lamp', 3, 34.99, 75), <br>
('Mouse', 1, 12.99, 100);**  <br>

-- Insert data into the "Customers" table. The involved columns are "first_name", "last_name", and "email". <br>
**INSERT INTO Customers (first_name, last_name, email) <br>
VALUES  <br>
('Ana', 'Popescu', 'ana.popescu@shopping.ro'), <br>
('George', 'Andrei', 'george.andrei@shopping.ro'), <br>
('Calin', 'Ionescu', 'calin.ionescu@shopping.ro'), <br>
('Ion', 'Cretu', 'ion.cretu@shopping.ro'), <br>
('Florina', 'Paler', 'florina.paler@shopping.ro');** <br>

-- Insert data into the "Orders" table. The involved columns are "customer_id", "order_date", "total_amount", and "order_status". 
Two orders are added to the "Orders" table, each order being associated with a specific customer through the "customer_id" column. <br>
**INSERT INTO Orders (customer_id, order_date, total_amount, order_status) <br>
VALUES <br>
(1, '2024-03-01', 299.98, 'completed'), <br>
(2, '2024-03-02', 39.98, 'processing'), <br>
(3, '2023-01-03', 500.00, 'completed'), <br>
(4, '2023-01-04', 200.00, 'shipped'), <br>
(5, '2023-01-05', 300.00, 'processing'), <br>
(1, '2023-01-06', 120.00, 'completed'), <br>
(2, '2023-01-07', 80.00, 'completed'), <br>
(3, '2023-01-08', 50.00, 'completed'), <br>
(4, '2023-01-09', 400.00, 'processing');** <br>

-- Insert three additional orders to the "Orders" table. Although the order status is not specified, they are considered implicitly as "pending" since it's not specified otherwise. 
These orders are associated with different customers and have different order dates and total amounts. <br>
**INSERT INTO Orders (customer_id, order_date, total_amount) <br>
VALUES (1, '2023-03-20', 120.50), (2, '2023-03-21', 85.75), (3, '2023-03-22', 200.00);** <br>

-- Insert records into the "OrderDetails" table. Each record is defined by the values specified within parentheses, with each set of values separated by a comma. <br>
**INSERT INTO OrderDetails (order_id, product_id, quantity, price) <br>
VALUES <br>
(1, 1, 2, 159.98), <br>
(2, 2, 3, 59.97), <br>
(3, 3, 1, 49.99), <br>
(4, 1, 1, 159.99), <br>
(4, 2, 1, 19.99), <br>
(4, 3, 2, 99.98), <br>
(5, 1, 3, 479.97), <br>
(6, 2, 4, 79.96), <br>
(7, 3, 2, 99.98), <br>
(8, 1, 2, 319.98), <br>
(8, 3, 3, 149.97), <br>
(9, 2, 2, 39.98), <br>
(10, 3, 1, 49.99);** <br>

  After the insert, in order to prepare the data to be better suited for the testing process, I updated some data in the following way:

-- Update the description of the category with the name 'Home Decor' in the Categories table. It changes the existing description of this category to 'Products for home decoration'. 
The WHERE clause specifies that the update applies only if the category name is 'Home Decor'. <br>
**UPDATE Categories <br>
SET description = 'Products for home decoration' <br>
WHERE category_name = 'Home Decor';** <br>

-- Update the price of the product with the name 'T-shirt' in the Products table. It changes the existing price of this product to 29.99. 
The WHERE clause specifies that the update applies only if the product name is 'T-shirt'. <br>
**UPDATE Products <br>
SET price = 29.99 <br>
WHERE product_name = 'T-shirt';** <br>

-- Update the category name with the id 3001 in the Categories table. It changes the existing name of this category to 'Electronics'. 
The WHERE clause specifies that the update applies only if the category id is 3001. <br>
**UPDATE Categories <br>
SET category_name = 'Electronics' <br>
WHERE category_id = 3001;** <br>

  <li>DQL (Data Query Language)</li> <br> 

After the testing process, I deleted the data that was no longer relevant in order to preserve the database clean: 

-- Delete the records from the Categories table where category_name is equal to 'Products for home decoration'. 
In other words, this will delete the category with the specified name from the Categories table. <br>
**DELETE FROM Categories <br>
WHERE category_name = 'Products for home decoration';** <br>

-- Delete the records from the Products table where product_name is equal to 'Table Lamp'. This will delete the specified product from the Products table. <br>
**DELETE FROM Products <br>
WHERE product_name = 'Table Lamp';** <br>

-- Delete the record from the Orders table where order_id is equal to 2. This will delete the order with the specified ID from the Orders table. <br>
**DELETE FROM Orders <br>
WHERE order_id = 2;** <br>

-- Delete all records from the Orders table where customer_id is equal to 2. This will delete all orders associated with the customer with the specified ID from the Orders table. <br>
**DELETE FROM Orders <br>
WHERE customer_id = 2;** <br>

-- Delete the record from the Customers table where customer_id is equal to 2. This will delete the customer with the specified ID from the Customers table. <br>
**DELETE FROM Customers <br>
WHERE customer_id = 2;** <br>

In order to simulate various scenarios that might happen in real life I created the following queries that would cover multiple potential real-life situations:

-- Select all products with a price greater than 100 and which are in the category with ID 1. <br>
```sql
SELECT * FROM Products <br>
WHERE price > 100 AND category_id = 1;
```

-- Select all orders that have an order date prior to January 1, 2023 or have a total amount less than 500. <br>
```sql
SELECT * FROM Orders WHERE order_date < '2023-01-01' OR total_amount < 500;
```

-- Select all products that are not in the category with ID 1. <br>
```sql
SELECT * FROM Products WHERE NOT category_id = 1;
```

-- Select all products whose names contain the word 'phone'. <br>
```sql
SELECT * FROM Products WHERE product_name LIKE '%phone%';
```

-- This query uses an inner join to combine the data from the Orders and OrderDetails tables based on the equal values of the order_id column. <br>
```sql
SELECT Orders.order_id, Orders.order_date, OrderDetails.product_id, OrderDetails.quantity
FROM Orders INNER JOIN OrderDetails ON Orders.order_id = OrderDetails.order_id;
```

-- This query uses a left join to display all records from the Orders table and only the records from the OrderDetails table that have a match with Orders. <br>
```sql
SELECT Orders.order_id, Orders.order_date, OrderDetails.product_id, OrderDetails.quantity
FROM Orders LEFT JOIN OrderDetails ON Orders.order_id = OrderDetails.order_id;
```

-- This query uses a right join to display all records from the OrderDetails table and only the records from the Orders table that have a match with OrderDetails. <br>
```sql
SELECT Orders.order_id, Orders.order_date, OrderDetails.product_id, OrderDetails.quantity
FROM Orders RIGHT JOIN OrderDetails ON Orders.order_id = OrderDetails.order_id;
```

-- This query performs a cross join between the Orders and Products tables, returning all possible combinations between the records from the two tables. <br>
```sql
SELECT Orders.order_id, Orders.order_date, Products.product_name, Products.price
FROM Orders CROSS JOIN Products;
```

-- This query calculates the number of products in each category and groups them by the category name. <br>
```sql
SELECT Categories.category_name, COUNT(Products.product_id) AS num_of_products
FROM Categories INNER JOIN Products ON Categories.category_id = Products.category_id
GROUP BY Categories.category_name;
```

-- This query calculates the number of products in each category and groups them by category name, but returns only the categories with more than 0 products. <br>
```sql
SELECT Categories.category_name, COUNT(Products.product_id) AS num_of_products 
FROM Categories 
INNER JOIN Products ON Categories.category_id = Products.category_id 
GROUP BY Categories.category_name 
HAVING COUNT(Products.product_id) > 0;
```

-- This query selects all orders for customers with the last name 'Popescu', using a subquery to obtain the IDs of the customers with this last name from the Customers table. <br>
```sql
SELECT * FROM Orders 
WHERE customer_id IN (SELECT customer_id FROM Customers WHERE last_name = 'Popescu');
```

</ol>

<li>Conclusions</li> <br>

**I learned how to design and implement databases, perform operations to create, update, and delete structures, and manipulate data using DDL, DML, and DQL statements.** <br>

**I worked with SQL queries to extract data and explored various techniques for filtering, joining, and aggregating data. I understood the importance of rigorous testing to ensure data integrity and consistency.** <br>

**In conclusion, this project was a valuable experience that helped me apply essential knowledge in database testing and management acquired during the testing course, preparing me for future projects and a career in software testing.** <br> 

</ol>
