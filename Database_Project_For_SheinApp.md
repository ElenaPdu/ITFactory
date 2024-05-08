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

-- Creeaza o noua baza de date numita SheinApp. <br>
**CREATE DATABASE SheinApp;** <br>
  
-- Creeaza tabela "Categories" cu coloanele "category_id", "category_name" si "description" utilizate pentru stocarea categoriilor de produse disponibile in aplicatie. <br>
**CREATE TABLE Categories (category_id INT AUTO_INCREMENT PRIMARY KEY, <br>
category_name VARCHAR(255) NOT NULL, description TEXT);** <br>
  

-- Creeaza tabela "Products" cu coloanele "product_id", "product_name", "category_id", "price" si "stock_quantity". 
"category_id" este o cheie externa care se refera la "category_id" din tabela "Categories". <br>
**CREATE TABLE Products (product_id INT AUTO_INCREMENT PRIMARY KEY, product_name VARCHAR(255) NOT NULL, category_id INT, price DECIMAL(10, 2), FOREIGN KEY (category_id) REFERENCES Categories(category_id));** <br>

-- Creeaza tabela "Customers" pentru a stoca informatiile despre clienti, inclusiv "customer_id", "first_name", "last_name" si "email". <br>
**CREATE TABLE Customers (customer_id INT AUTO_INCREMENT PRIMARY KEY, <br>
first_name VARCHAR(50), last_name VARCHAR(50), email VARCHAR(100));** <br>


-- Creeaza tabela "Orders" pentru a gestiona comenzile clientilor, inclusiv "order_id", "customer_id", "order_date" si "total_amount". <br>
**CREATE TABLE Orders (order_id INT AUTO_INCREMENT PRIMARY KEY, customer_id INT, order_date DATE, total_amount DECIMAL(10, 2), FOREIGN KEY (customer_id) REFERENCES Customers(customer_id));** <br>

-- Creeaza tabela "OrderDetails" pentru a tine evidenta detaliilor comenzilor, inclusiv "order_detail_id", "order_id", "product_id", "quantity" si "price". 
"order_id" si "product_id" sunt chei externe care se refera la "order_id" si "product_id" din tabelele "Orders" si "Products". <br>
**CREATE TABLE OrderDetails (order_detail_id INT AUTO_INCREMENT PRIMARY KEY, order_id INT, product_id INT, quantity INT, price DECIMAL(10, 2), FOREIGN KEY (order_id) REFERENCES Orders(order_id), FOREIGN KEY (product_id) REFERENCES Products(product_id));** <br>

  After the database and the tables have been created, a few ALTER instructions were written in order to update the structure of the database, as described below:

-- Adauga o noua coloana "stock_quantity" la tabela "Products" pentru a tine evidenta stocului disponibil pentru fiecare produs. <br>
**ALTER TABLE Products <br>
ADD COLUMN stock_quantity INT AFTER price;** <br>

-- Adauga o noua coloana "category_code" la tabela "Categories" pentru a stoca un cod unic pentru fiecare categorie. <br>
**ALTER TABLE Categories <br>
ADD COLUMN category_code VARCHAR(10) AFTER description;** <br>

-- Schimba numele coloanei "stock_quantity" in "available_quantity" pentru a face schema mai explicita. <br>
**ALTER TABLE Products <br>
CHANGE COLUMN stock_quantity available_quantity INT;** <br>

-- Adauga o noua coloana "order_status" la tabela "Orders" cu valoarea implicita "pending" pentru a tine evidenta starii comenzilor. <br>
**ALTER TABLE Orders <br>
ADD COLUMN order_status VARCHAR(20) DEFAULT 'pending';** <br>

-- Modifica lungimea coloanei "email" din "VARCHAR(100)" in "VARCHAR(255)" pentru a permite adrese de email mai lungi. <br>
**ALTER TABLE Customers <br>
MODIFY COLUMN email VARCHAR(255);** <br>
 
  
  <li>DML (Data Manipulation Language)</li> <br>

  In order to be able to use the database I populated the tables with various data necessary in order to perform queries and manipulate the data. 
  In the testing process, this necessary data is identified in the Test Design phase and created in the Test Implementation phase. 

  Below you can find all the insert instructions that were created in the scope of this project:

-- Insereaza date in tabela "Categories". Coloanele implicate sunt "category_name" si "description". 
Se adauga trei randuri in tabela "Categories", fiecare rand reprezentand o categorie diferita de produse: "Electronics" pentru dispozitive si accesorii electronice, 
"Clothing" pentru articole de imbracaminte pentru barbati, femei si copii, si "Home Decor" pentru obiecte decorative pentru designul interior al casei. <br>
**INSERT INTO Categories (category_name, description) <br>
VALUES <br>
('Electronics', 'Products related to electronic devices and accessories'), <br>
('Clothing', 'Clothing items for men, women, and children'), <br>
('Home Decor', 'Decorative items for home interior design');** <br>

-- Insereaza date in tabela "Products". Coloanele implicate sunt "product_name", "category_id", "price" si "available_quantity". 
Se adauga trei produse in tabela "Products", fiecare fiind asociat cu o anumita categorie prin intermediul coloanei "category_id". <br>
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

-- Insereaza date in tabela "Customers". Coloanele implicate sunt "first_name", "last_name" si "email". <br>
**INSERT INTO Customers (first_name, last_name, email) <br>
VALUES  <br>
('Ana', 'Popescu', 'ana.popescu@shopping.ro'), <br>
('George', 'Andrei', 'george.andrei@shopping.ro'), <br>
('Calin', 'Ionescu', 'calin.ionescu@shopping.ro'), <br>
('Ion', 'Cretu', 'ion.cretu@shopping.ro'), <br>
('Florina', 'Paler', 'florina.paler@shopping.ro');** <br>

-- Insereaza date in tabela "Orders". Coloanele implicate sunt "customer_id", "order_date", "total_amount" si "order_status". 
Se adauga doua comenzi in tabela "Orders", fiecare comanda fiind asociata cu un anumit client prin intermediul coloanei "customer_id". <br>
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

-- Adauga trei comenzi suplimentare in tabela "Orders". Desi nu este specificata starea comenzilor, acestea sunt considerate implicit ca fiind in "pending", deoarece nu este specificat altfel. 
Aceste comenzi sunt asociate cu diferiti clienti si au date de comanda si sume totale diferite. <br>
**INSERT INTO Orders (customer_id, order_date, total_amount) <br>
VALUES (1, '2023-03-20', 120.50), (2, '2023-03-21', 85.75), (3, '2023-03-22', 200.00);** <br>

-- Adauga inregistrari in tabela "OrderDetails". Fiecare inregistrare este definita de valorile specificate intre paranteze, cu fiecare set de valori separat prin virgula. <br>
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

-- Actualizeaza descrierea categoriei cu numele 'Home Decor' in tabelul Categories. Ea schimba descrierea existenta a acestei categorii in 'Products for home decoration'. 
Clauza WHERE specifica ca actualizarea se aplica numai in cazul in care numele categoriei este 'Home Decor'. <br>
**UPDATE Categories <br>
SET description = 'Products for home decoration' <br>
WHERE category_name = 'Home Decor';** <br>

-- Actualizeaza pretul produsului cu numele 'T-shirt' in tabelul Products. Ea schimba pretul existent al acestui produs in 29.99. 
Clauza WHERE specifica ca actualizarea se aplica numai in cazul in care numele produsului este 'T-shirt'. <br>
**UPDATE Products <br>
SET price = 29.99 <br>
WHERE product_name = 'T-shirt';** <br>

-- Actualizeaza numele categoriei cu id-ul 3001 in tabelul Categories. Ea schimba numele existent al acestei categorii in 'Electronics'. 
Clauza WHERE specifica ca actualizarea se aplica numai in cazul in care id-ul categoriei este 3001. <br>
**UPDATE Categories <br>
SET category_name = 'Electronics' <br>
WHERE category_id = 3001;** <br>

  <li>DQL (Data Query Language)</li> <br> 

After the testing process, I deleted the data that was no longer relevant in order to preserve the database clean: 

-- Sterge inregistrarile din tabela Categories unde category_name este egal cu 'Products for home decoration'. 
Cu alte cuvinte, aceasta va sterge categoria cu numele specificat din tabelul Categories. <br>
**DELETE FROM Categories <br>
WHERE category_name = 'Products for home decoration';** <br>

-- Sterge inregistrarile din tabela Products unde product_name este egal cu 'Table Lamp'. Va sterge produsul specificat din tabelul Products. <br>
**DELETE FROM Products <br>
WHERE product_name = 'Table Lamp';** <br>

-- Sterge inregistrarea din tabela Orders unde order_id este egal cu 2. Va sterge comanda cu ID-ul specificat din tabelul Orders. <br>
**DELETE FROM Orders <br>
WHERE order_id = 2;** <br>

-- Sterge toate inregistrarile din tabela Orders care au customer_id egal cu 2. Va sterge toate comenzile asociate clientului cu ID-ul specificat din tabelul Orders. <br>
**DELETE FROM Orders <br>
WHERE customer_id = 2;** <br>

-- Sterge inregistrarea din tabela Customers unde customer_id este egal cu 2. Va sterge clientul cu ID-ul specificat din tabelul Customers. <br>
**DELETE FROM Customers <br>
WHERE customer_id = 2;** <br>

In order to simulate various scenarios that might happen in real life I created the following queries that would cover multiple potential real-life situations:

-- Selecteaza toate produsele cu un pret mai mare de 100 si care sunt in categoria cu ID-ul 1. <br>
**SELECT * FROM Products <br>
WHERE price > 100 AND category_id = 1;** <br>

-- Selecteaza toate comenzile care au data de comanda anterioara datei de 1 ianuarie 2023 sau care au un total mai mic de 500. <br>
**SELECT * FROM Orders <br>
WHERE order_date < '2023-01-01' OR total_amount < 500;** <br>

-- Selecteaza toate produsele care nu sunt in categoria cu ID-ul 1. <br>
**SELECT * FROM Products <br>
WHERE NOT category_id = 1;** <br>

-- Selecteaza toate produsele ale caror nume contine cuvantul 'phone'. <br>
**SELECT * FROM Products <br>
WHERE product_name LIKE '%phone%';** <br>

-- Aceasta interogare foloseste un inner join pentru a combina datele din tabelele Orders si OrderDetails pe baza valorilor egale ale coloanei order_id. <br>
**SELECT Orders.order_id, Orders.order_date, OrderDetails.product_id, OrderDetails.quantity <br>
FROM Orders <br>
INNER JOIN OrderDetails ON Orders.order_id = OrderDetails.order_id;** <br>

-- Aceasta interogare foloseste un left join pentru a afisa toate inregistrarile din tabela Orders si doar inregistrările din OrderDetails care au corespondenta cu Orders. <br>
**SELECT Orders.order_id, Orders.order_date, OrderDetails.product_id, OrderDetails.quantity <br>
FROM Orders <br>
LEFT JOIN OrderDetails ON Orders.order_id = OrderDetails.order_id;** <br>

-- Aceasta interogare foloseste un right join pentru a afisa toate inregistrarile din tabela OrderDetails si doar inregistrarile din Orders care au corespondenta cu OrderDetails. <br>
**SELECT Orders.order_id, Orders.order_date, OrderDetails.product_id, OrderDetails.quantity <br>
FROM Orders <br>
RIGHT JOIN OrderDetails ON Orders.order_id = OrderDetails.order_id;** <br>

-- Aceasta interogare efectueaza un cross join intre tabelele Orders si Products, returnand toate combinatiile posibile intre inregistrarile din cele doua tabele. <br>
**SELECT Orders.order_id, Orders.order_date, Products.product_name, Products.price <br>
FROM Orders <br>
CROSS JOIN Products;** <br> 

-- Aceasta interogare calculeaza numarul de produse din fiecare categorie si le grupeaza dupa numele categoriei. <br>
**SELECT Categories.category_name, COUNT(Products.product_id) AS num_of_products <br>
FROM Categories <br>
INNER JOIN Products ON Categories.category_id = Products.category_id <br>
GROUP BY Categories.category_name;** <br>

-- Aceasta interogare calculeaza numarul de produse din fiecare categorie si le grupeaza dupa nume de categorie, dar returneaza doar categoriile cu mai mult de 0 produse. <br>
**SELECT Categories.category_name, COUNT(Products.product_id) AS num_of_products <br>
FROM Categories <br>
INNER JOIN Products ON Categories.category_id = Products.category_id <br>
GROUP BY Categories.category_name <br>
HAVING COUNT(Products.product_id) > 0;** <br>

-- Aceasta interogare selecteaza toate comenzile pentru clientii cu numele de familie 'Popescu', folosind o subinterogare pentru a obtine id-urile clientilor cu acest nume de familie din tabela Customers. <br>
**SELECT * FROM Orders <br>
WHERE customer_id IN (SELECT customer_id FROM Customers WHERE last_name = 'Popescu');** <br>

</ol>

<li>Conclusions</li> <br>

**Am invatat sa proiectez si sa implementez baze de date, sa fac operatii de creare, actualizare si stergere a structurii si sa manipulez datele folosind instructiuni DDL, DML si DQL.** <br>

**Am lucrat cu interogari SQL pentru a extrage date si am explorat diverse tehnici de filtrare, imbinare si agregare a datelor. Am inteles importanta testarii riguroase pentru asigurarea integritatii si consistentei datelor.** <br>

**In concluzie, acest proiect a fost o experienta valoroasa care m-a ajutat sa aplic cunostintele esentiale in testarea si gestionarea bazelor de date dobandite in cadrul cursului de testare, pregatindu-ma pentru viitoarele proiecte si cariera in testarea software.** <br> 

</ol>
