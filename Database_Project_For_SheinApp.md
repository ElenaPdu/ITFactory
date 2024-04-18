<h1>Database Project for SheinApp </h1>

The scope of this project is to use all the SQL knowledge gained throught the Software Testing course and apply them in practice.

Application under test: **SheinApp**

Tools used: MySQL Workbench

Database description: **The database supports SheinApp, an e-commerce platform for fashion products. It stores information about products, categories, customers, orders, and order details. This facilitates inventory management, customer accounts, order processing, and order details.**

<ol>
<li>Database Schema </li>
<br>
You can find below the database schema that was generated through Reverse Engineer and which contains all the tables and the relationships between them.

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

**CREATE DATABASE SheinApp;** <br>
  
**CREATE TABLE Categories (category_id INT AUTO_INCREMENT PRIMARY KEY, <br>
category_name VARCHAR(255) NOT NULL, description TEXT);** <br>
  
**CREATE TABLE Products (product_id INT AUTO_INCREMENT PRIMARY KEY, product_name VARCHAR(255) NOT NULL, category_id INT, price DECIMAL(10, 2), FOREIGN KEY (category_id) REFERENCES Categories(category_id));** <br>

**CREATE TABLE Customers (customer_id INT AUTO_INCREMENT PRIMARY KEY, <br>
first_name VARCHAR(50), last_name VARCHAR(50), email VARCHAR(100));** <br>

**CREATE TABLE Orders (order_id INT AUTO_INCREMENT PRIMARY KEY, customer_id INT, order_date DATE, total_amount DECIMAL(10, 2), FOREIGN KEY (customer_id) REFERENCES Customers(customer_id));** <br>

**CREATE TABLE OrderDetails (order_detail_id INT AUTO_INCREMENT PRIMARY KEY, order_id INT, product_id INT, quantity INT, price DECIMAL(10, 2), FOREIGN KEY (order_id) REFERENCES Orders(order_id), FOREIGN KEY (product_id) REFERENCES Products(product_id));** <br>

  After the database and the tables have been created, a few ALTER instructions were written in order to update the structure of the database, as described below:

**ALTER TABLE Products <br>
ADD COLUMN stock_quantity INT AFTER price;** <br>

**ALTER TABLE Categories <br>
ADD COLUMN category_code VARCHAR(10) AFTER description;** <br>

**ALTER TABLE Products <br>
CHANGE COLUMN stock_quantity available_quantity INT;** <br>

**ALTER TABLE Orders <br>
ADD COLUMN order_status VARCHAR(20) DEFAULT 'pending';** <br>

**ALTER TABLE Customers <br>
MODIFY COLUMN email VARCHAR(255);** <br>
 
  
  <li>DML (Data Manipulation Language)</li> <br>

  In order to be able to use the database I populated the tables with various data necessary in order to perform queries and manipulate the data. 
  In the testing process, this necessary data is identified in the Test Design phase and created in the Test Implementation phase. 

  Below you can find all the insert instructions that were created in the scope of this project:

**INSERT INTO Categories (category_name, description) <br>
VALUES <br>
('Electronics', 'Products related to electronic devices and accessories'), <br>
('Clothing', 'Clothing items for men, women, and children'), <br>
('Home Decor', 'Decorative items for home interior design');** <br>

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

**INSERT INTO Customers (first_name, last_name, email) <br>
VALUES  <br>
('Ana', 'Popescu', 'ana.popescu@shopping.ro'), <br>
('George', 'Andrei', 'george.andrei@shopping.ro'), <br>
('Calin', 'Ionescu', 'calin.ionescu@shopping.ro'), <br>
('Ion', 'Cretu', 'ion.cretu@shopping.ro'), <br>
('Florina', 'Paler', 'florina.paler@shopping.ro');** <br>

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

**INSERT INTO Orders (customer_id, order_date, total_amount) <br>
VALUES (1, '2023-03-20', 120.50), (2, '2023-03-21', 85.75), (3, '2023-03-22', 200.00);** <br>

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

**UPDATE Categories <br>
SET description = 'Products for home decoration' <br>
WHERE category_name = 'Home Decor';** <br>

**UPDATE Products <br>
SET price = 29.99 <br>
WHERE product_name = 'T-shirt';** <br>

**UPDATE Categories <br>
SET category_name = 'Electronics' <br>
WHERE category_id = 3001;** <br>

  <li>DQL (Data Query Language)</li> <br> 

After the testing process, I deleted the data that was no longer relevant in order to preserve the database clean: 

**DELETE FROM Categories <br>
WHERE category_name = 'Products for home decoration';** <br>

**DELETE FROM Products <br>
WHERE product_name = 'Table Lamp';** <br>

**DELETE FROM Orders <br>
WHERE order_id = 2;** <br>

**DELETE FROM Orders <br>
WHERE customer_id = 2;** <br>

**DELETE FROM Customers <br>
WHERE customer_id = 2;** <br>

In order to simulate various scenarios that might happen in real life I created the following queries that would cover multiple potential real-life situations:

**SELECT * FROM Products <br>
WHERE price > 100 AND category_id = 1;** <br>

**SELECT * FROM Orders <br>
WHERE order_date < '2023-01-01' OR total_amount < 500;** <br>

**SELECT * FROM Products <br>
WHERE NOT category_id = 1;** <br>

**SELECT * FROM Products <br>
WHERE product_name LIKE '%phone%';** <br>

**SELECT Orders.order_id, Orders.order_date, OrderDetails.product_id, OrderDetails.quantity <br>
FROM Orders <br>
INNER JOIN OrderDetails ON Orders.order_id = OrderDetails.order_id;** <br>

**SELECT Orders.order_id, Orders.order_date, OrderDetails.product_id, OrderDetails.quantity <br>
FROM Orders <br>
LEFT JOIN OrderDetails ON Orders.order_id = OrderDetails.order_id;** <br>

**SELECT Orders.order_id, Orders.order_date, OrderDetails.product_id, OrderDetails.quantity <br>
FROM Orders <br>
RIGHT JOIN OrderDetails ON Orders.order_id = OrderDetails.order_id;** <br>

**SELECT Orders.order_id, Orders.order_date, Products.product_name, Products.price <br>
FROM Orders <br>
CROSS JOIN Products;** <br> 

**SELECT Categories.category_name, COUNT(Products.product_id) AS num_of_products <br>
FROM Categories <br>
INNER JOIN Products ON Categories.category_id = Products.category_id <br>
GROUP BY Categories.category_name;** <br>

**SELECT Categories.category_name, COUNT(Products.product_id) AS num_of_products <br>
FROM Categories <br>
INNER JOIN Products ON Categories.category_id = Products.category_id <br>
GROUP BY Categories.category_name <br>
HAVING COUNT(Products.product_id) > 0;** <br>

**SELECT * FROM Orders <br>
WHERE customer_id IN (SELECT customer_id FROM Customers WHERE last_name = 'Popescu');** <br>

</ol>

<li>Conclusions</li> <br>

**Am invatat sa proiectez si sa implementez baze de date, sa fac operatii de creare, actualizare si stergere a structurii si sa manipulez datele folosind instructiuni DDL, DML si DQL.** <br>

**Am lucrat cu interogari SQL pentru a extrage date si am explorat diverse tehnici de filtrare, imbinare si agregare a datelor. Am inteles importanta testarii riguroase pentru asigurarea integritatii si consistentei datelor.** <br>

**In concluzie, acest proiect a fost o experienta valoroasa care m-a ajutat sa aplic cunostintele esentiale in testarea si gestionarea bazelor de date dobandite in cadrul cursului de testare, pregatindu-ma pentru viitoarele proiecte si cariera in testarea software.** <br> 

</ol>
