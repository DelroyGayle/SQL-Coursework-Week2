# E-Commerce Database

In this homework, you are going to work with an ecommerce database. In this database, you have `products` that `consumers` can buy from different `suppliers`. Customers can create an `order` and several products can be added in one order.

## Submission

Below you will find a set of tasks for you to complete to set up a database for an e-commerce app.

To submit this homework write the correct commands for each question here:
```sql

1. Retrieve all the customers' names and addresses who live in the United States
SELECT name, address FROM customers
WHERE country = 'United States';

     name     |          address           
--------------+----------------------------
 Amber Tran   | 6967 Ac Road
 Edan Higgins | Ap #840-3255 Tincidunt St.
(2 rows)

2. Retrieve all the customers in ascending name sequence

SELECT * FROM customers
ORDER BY name ASC;

 id |        name        |           address           |       city       |    country     
----+--------------------+-----------------------------+------------------+----------------
  4 | Amber Tran         | 6967 Ac Road                | Villafranca Asti | United States
  3 | Britanney Kirkland | P.O. Box 577, 5601 Sem, St. | Little Rock      | United Kingdom
  5 | Edan Higgins       | Ap #840-3255 Tincidunt St.  | Arles            | United States
  1 | Guy Crawford       | 770-2839 Ligula Road        | Paris            | France
  2 | Hope Crosby        | P.O. Box 276, 4976 Sit Rd.  | Steyr            | United Kingdom
  6 | Quintessa Austin   | 597-2737 Nunc Rd.           | Saint-Marc       | United Kingdom
(6 rows)

3. Retrieve all the products whose name contains the word `socks`

SELECT * FROM products WHERE product_name LIKE '%socks%';

 id |   product_name   
----+------------------
  4 | Super warm socks
(1 row)

4. Retrieve all the products which cost more than 100 showing product id, name, unit price and supplier id.

 SELECT pa.prod_id, p.product_name, pa.unit_price, pa.supp_id
 FROM products AS p
 JOIN product_availability AS pa 
 ON p.id = pa.prod_id 
 AND pa.unit_price > 100;
 
  prod_id |  product_name  | unit_price | supp_id 
---------+----------------+------------+---------
       1 | Mobile Phone X |        249 |       4
       1 | Mobile Phone X |        299 |       1
(2 rows)

5. Retrieve the 5 most expensive products

 SELECT *
 FROM products AS p
 JOIN product_availability AS pa 
 ON p.id = pa.prod_id 
 ORDER BY unit_price DESC
 LIMIT 5;
 
 id |  product_name   | prod_id | supp_id | unit_price 
----+-----------------+---------+---------+------------
  1 | Mobile Phone X  |       1 |       1 |        299
  1 | Mobile Phone X  |       1 |       4 |        249
  2 | Javascript Book |       2 |       2 |         41
  2 | Javascript Book |       2 |       1 |         40
  2 | Javascript Book |       2 |       3 |         39

6. Retrieve all the products with their corresponding suppliers.
The result should only contain the columns `product_name`, `unit_price` and `supplier_name`

SELECT DISTINCT p.product_name, pa.unit_price, s.supplier_name
 FROM products AS p
 JOIN product_availability AS pa 
 ON p.id = pa.prod_id 
 JOIN suppliers AS s
 ON p.id = s.id
 ORDER BY p.product_name; 
 
   product_name   | unit_price | supplier_name 
------------------+------------+---------------
 Javascript Book  |         39 | Taobao
 Javascript Book  |         40 | Taobao
 Javascript Book  |         41 | Taobao
 Le Petit Prince  |         10 | Argos
 Mobile Phone X   |        249 | Amazon
 Mobile Phone X   |        299 | Amazon
 Super warm socks |          5 | Sainsburys
 Super warm socks |          8 | Sainsburys
 Super warm socks |         10 | Sainsburys
(9 rows)
 
7. Retrieve all the products sold by suppliers based in the United Kingdom.
The result should only contain the columns `product_name` and `supplier_name`.

SELECT p.product_name, s.supplier_name
 FROM products AS p,
 product_availability AS pa, 
 suppliers AS s
 WHERE p.id = pa.prod_id AND pa.supp_id = s.id AND s.country = 'United Kingdom'
 ORDER BY p.product_name, s.supplier_name;

      product_name       | supplier_name 
-------------------------+---------------
 Ball                    | Sainsburys
 Coffee Cup              | Argos
 Coffee Cup              | Sainsburys
 Javascript Book         | Argos
 Le Petit Prince         | Sainsburys
 Mobile Phone X          | Sainsburys
 Super warm socks        | Argos
 Super warm socks        | Sainsburys
 Tee Shirt Olympic Games | Argos
(9 rows)
 
8. Retrieve all orders, including order items, from customer ID `1`.
Include order id, reference, date and total cost (calculated as quantity * unit price).

SELECT order_id AS "order id", order_reference AS reference, order_date AS date, quantity * unit_price AS "total cost"
FROM order_items
JOIN orders
ON orders.id = order_id
JOIN product_availability
ON product_id = prod_id
WHERE customer_id = 1 AND supplier_id = supp_id;

 order id | reference |    date    | total cost 
----------+-----------+------------+------------
        1 | ORD001    | 2019-06-01 |         18
        1 | ORD001    | 2019-06-01 |         25
        2 | ORD002    | 2019-07-15 |         32
        2 | ORD002    | 2019-07-15 |         10
        3 | ORD003    | 2019-07-11 |         40
        3 | ORD003    | 2019-07-11 |         40
(6 rows)

9. Retrieve all orders, including order items, from customer named `Hope Crosby`

SELECT orders.order_date, orders.order_reference, orders.customer_id, order_id, product_id, supplier_id, quantity 
FROM order_items
JOIN customers
ON customers.name = 'Hope Crosby'
JOIN orders
ON customer_id = customers.id
WHERE customers.name = 'Hope Crosby';

<< or >>

SELECT orders.order_date, orders.order_reference, orders.customer_id, order_id, product_id, supplier_id, quantity 
FROM order_items,
customers,
orders
WHERE customers.name = 'Hope Crosby' AND customer_id = customers.id;

 order_date | order_reference | customer_id | order_id | product_id | supplier_id | quantity 
------------+-----------------+-------------+----------+------------+-------------+----------
 2019-05-24 | ORD004          |           2 |        1 |          7 |           2 |        1
 2019-05-24 | ORD004          |           2 |        1 |          4 |           2 |        5
 2019-05-24 | ORD004          |           2 |        2 |          4 |           3 |        4
 2019-05-24 | ORD004          |           2 |        2 |          3 |           4 |        1
 2019-05-24 | ORD004          |           2 |        3 |          5 |           3 |       10
 2019-05-24 | ORD004          |           2 |        3 |          6 |           2 |        2
 2019-05-24 | ORD004          |           2 |        4 |          1 |           1 |        1
 2019-05-24 | ORD004          |           2 |        5 |          2 |           3 |        2
 2019-05-24 | ORD004          |           2 |        5 |          3 |           1 |        1
 2019-05-24 | ORD004          |           2 |        6 |          5 |           2 |        3
 2019-05-24 | ORD004          |           2 |        6 |          2 |           2 |        1
 2019-05-24 | ORD004          |           2 |        6 |          3 |           4 |        1
 2019-05-24 | ORD004          |           2 |        6 |          4 |           4 |        3
 2019-05-24 | ORD004          |           2 |        7 |          4 |           3 |       15
 2019-05-24 | ORD004          |           2 |        8 |          7 |           1 |        1
 2019-05-24 | ORD004          |           2 |        8 |          1 |           4 |        1
 2019-05-24 | ORD004          |           2 |        9 |          6 |           4 |        2
 2019-05-24 | ORD004          |           2 |       10 |          6 |           2 |        1
 2019-05-24 | ORD004          |           2 |       10 |          4 |           1 |        5
(19 rows)

10. Retrieve all the products in the order `ORD006`. 
The result should only contain the columns `product_name`, `unit_price` and `quantity`.

SELECT product_name, unit_price, quantity                       
FROM orders
JOIN order_items
ON order_reference = 'ORD006' AND orders.id = order_id
JOIN product_availability
ON product_id = prod_id AND supplier_id = supp_id
JOIN products
ON product_id = products.id;

   product_name   | unit_price | quantity 
------------------+------------+----------
 Coffee Cup       |          4 |        3
 Javascript Book  |         41 |        1
 Le Petit Prince  |         10 |        1
 Super warm socks |         10 |        3
(4 rows)

11. Retrieve all the products with their supplier for all orders of all customers. 
The result should only contain the columns `name` (from customer), `order_reference`, 
                                           `order_date`, `product_name`, `supplier_name` and `quantity`.

SELECT customers.name, order_reference, order_date, product_name, quantity 
FROM customers
JOIN orders
ON customer_id = customers.id
JOIN order_items
ON orders.id = order_id
JOIN product_availability
ON product_id = prod_id AND supplier_id = supp_id
JOIN products
ON product_id = products.id
JOIN suppliers
ON supp_id = suppliers.id
ORDER BY customers.name, order_reference;

        name        | order_reference | order_date |      product_name       | quantity 
--------------------+-----------------+------------+-------------------------+----------
 Amber Tran         | ORD006          | 2019-07-05 | Coffee Cup              |        3
 Amber Tran         | ORD006          | 2019-07-05 | Super warm socks        |        3
 Amber Tran         | ORD006          | 2019-07-05 | Le Petit Prince         |        1
 Amber Tran         | ORD006          | 2019-07-05 | Javascript Book         |        1
 Amber Tran         | ORD007          | 2019-04-05 | Super warm socks        |       15
 Britanney Kirkland | ORD005          | 2019-05-30 | Javascript Book         |        2
 Britanney Kirkland | ORD005          | 2019-05-30 | Le Petit Prince         |        1
 Edan Higgins       | ORD008          | 2019-07-23 | Mobile Phone X          |        1
 Edan Higgins       | ORD008          | 2019-07-23 | Tee Shirt Olympic Games |        1
 Edan Higgins       | ORD009          | 2019-07-24 | Ball                    |        2
 Edan Higgins       | ORD010          | 2019-05-10 | Super warm socks        |        5
 Edan Higgins       | ORD010          | 2019-05-10 | Ball                    |        1
 Guy Crawford       | ORD001          | 2019-06-01 | Tee Shirt Olympic Games |        1
 Guy Crawford       | ORD001          | 2019-06-01 | Super warm socks        |        5
 Guy Crawford       | ORD002          | 2019-07-15 | Super warm socks        |        4
 Guy Crawford       | ORD002          | 2019-07-15 | Le Petit Prince         |        1
 Guy Crawford       | ORD003          | 2019-07-11 | Ball                    |        2
 Guy Crawford       | ORD003          | 2019-07-11 | Coffee Cup              |       10
 Hope Crosby        | ORD004          | 2019-05-24 | Mobile Phone X          |        1
(19 rows)

12. Retrieve the names of all customers who bought a product from a supplier based in China.

SELECT DISTINCT name
FROM customers
JOIN orders
ON customer_id = customers.id
JOIN order_items
ON orders.id = order_id
JOIN suppliers
ON suppliers.country = 'China' AND supplier_id = suppliers.id
ORDER BY name;

<< or >>

SELECT DISTINCT name
FROM customers
JOIN orders
ON customer_id = customers.id
JOIN order_items
ON orders.id = order_id
WHERE supplier_id = 2
ORDER BY name;

     name     
--------------
 Amber Tran
 Edan Higgins
 Guy Crawford
(3 rows)


13. List all orders giving customer name, order reference, order date and 
                           order total amount (quantity * unit price) in descending order of total.

SELECT customers.name AS "customer name", orders.order_reference AS "order reference", 
    orders.order_date AS "order date", quantity * unit_price AS "order total amount"
FROM order_items
JOIN orders
ON orders.id = order_id
JOIN customers
ON customer_id = customers.id
JOIN products
ON product_id = products.id
JOIN product_availability
ON product_id = prod_id AND supplier_id = supp_id
ORDER BY quantity * unit_price DESC;

   customer name    | order reference | order date | order total amount 
--------------------+-----------------+------------+--------------------
 Hope Crosby        | ORD004          | 2019-05-24 |                299
 Edan Higgins       | ORD008          | 2019-07-23 |                249
 Amber Tran         | ORD007          | 2019-04-05 |                120
 Britanney Kirkland | ORD005          | 2019-05-30 |                 78
 Edan Higgins       | ORD010          | 2019-05-10 |                 50
 Amber Tran         | ORD006          | 2019-07-05 |                 41
 Guy Crawford       | ORD003          | 2019-07-11 |                 40
 Guy Crawford       | ORD003          | 2019-07-11 |                 40
 Guy Crawford       | ORD002          | 2019-07-15 |                 32
 Edan Higgins       | ORD009          | 2019-07-24 |                 30
 Amber Tran         | ORD006          | 2019-07-05 |                 30
 Guy Crawford       | ORD001          | 2019-06-01 |                 25
 Edan Higgins       | ORD008          | 2019-07-23 |                 20
 Edan Higgins       | ORD010          | 2019-05-10 |                 20
 Guy Crawford       | ORD001          | 2019-06-01 |                 18
 Amber Tran         | ORD006          | 2019-07-05 |                 12
 Amber Tran         | ORD006          | 2019-07-05 |                 10
 Britanney Kirkland | ORD005          | 2019-05-30 |                 10
 Guy Crawford       | ORD002          | 2019-07-15 |                 10
(19 rows)



```

When you have finished all of the questions - open a pull request with your answers to the `Databases-Homework` repository.

## Setup

To prepare your environment for this homework, open a terminal and create a new database called `cyf_ecommerce`:

```sql
createdb cyf_ecommerce
```

Import the file [`cyf_ecommerce.sql`](./cyf_ecommerce.sql) in your newly created database:

```sql
psql -d cyf_ecommerce -f cyf_ecommerce.sql
```

Open the file `cyf_ecommerce.sql` in VSCode and examine the SQL code. Take a piece of paper and draw the database with the different relationships between tables (as defined by the REFERENCES keyword in the CREATE TABLE commands). Identify the foreign keys and make sure you understand the full database schema.

## Task

Once you understand the database that you are going to work with, solve the following challenge by writing SQL queries using everything you learned about SQL:

1. Retrieve all the customers' names and addresses who live in the United States
SELECT name, address FROM customers
WHERE country 
2. Retrieve all the customers in ascending name sequence
3. Retrieve all the products whose name contains the word `socks`
4. Retrieve all the products which cost more than 100 showing product id, name, unit price and supplier id.
5. Retrieve the 5 most expensive products
6. Retrieve all the products with their corresponding suppliers. The result should only contain the columns `product_name`, `unit_price` and `supplier_name`
7. Retrieve all the products sold by suppliers based in the United Kingdom. The result should only contain the columns `product_name` and `supplier_name`.
8. Retrieve all orders, including order items, from customer ID `1`. Include order id, reference, date and total cost (calculated as quantity * unit price).
9. Retrieve all orders, including order items, from customer named `Hope Crosby`
10. Retrieve all the products in the order `ORD006`. The result should only contain the columns `product_name`, `unit_price` and `quantity`.
11. Retrieve all the products with their supplier for all orders of all customers. The result should only contain the columns `name` (from customer), `order_reference`, `order_date`, `product_name`, `supplier_name` and `quantity`.
12. Retrieve the names of all customers who bought a product from a supplier based in China.
13. List all orders giving customer name, order reference, order date and order total amount (quantity * unit price) in descending order of total.

