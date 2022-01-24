# SQL Queries
## Retrieving columns
```
SELECT name FROM products;
```
Returns the _name_ column  of all the entries from the table _products_.
____
```
SELECT name, price FROM products;
```
Returns _name_ and _price_ columns from the table _products_.
* If you need to select multiple columns, you can mention them seperated by commas
* The returned table will contain the columns in the order mentioned in the command
____

```
SELECT * FROM products;
```
This command selects **all** the entries from the table _products_.

## Using aliases
```
SELECT id AS products_id, name AS product_name FROM products;
```
We use the **AS** keyword to give column names aliases.

## Filtering entries
```
SELECT * FROM products
WHERE name = 'TV';
```
Returns the  entries from the table _products_ with the name _TV_.
___
```
SELECT * FROM products
WHERE price < 100;
```
Returns all the products whose price is less than 100
* Similarly, we can use any comparison operators: <, >, <=, >=, != or <>

## Multiple conditions
```
SELECT * from products
WHERE inventory > 0 AND price > 20;
```
Returns products whose inventory is greater than 0 **and** price more than 20. 
___
```
SELECT * from  products
WHERE price > 100 OR price < 20;
```
Returns the products whose price is less than 100 **or** more than 20.

## IN Operator
```
SELECT * FROM products
WHERE id IN (1, 2, 3);
```
This command returns the same result when the command below is run
```
SELECT * FROM PRODUCTS
WHERE id=1 OR id=2 OR id=3;
```

## LIKE Operator
```
SELECT * FROM PRODUCTS
WHERE name LIKE 'TV%';
```
Returns every row that has a name which **starts with** 'TV'
___
```
SELECT * FROM PRODUCTS
WHERE name LIKE '%e';
```
Returns every row that has a name which **ends with** the letter 'e'
___
```
SELECT * FROM PRODUCTS
WHERE name NOT LIKE '%e';
```
Returns every row that has a name which does **not end with** the letter 'e'
___
```
SELECT * FROM PRODUCTS
WHERE name LIKE '%er%';
```
Returns every row that has a name which has 'er' anywhere in between

## ORDER BY
```
SELECT * FROM products
ORDER BY price;
```
Returns entries from _products_ table, in an ascending order of price.
* By default, it displays in the **ascending order**. But we can use the **DESC** keyword to display the same in descending order.
* We also can use the **ASC** keyword if we'd like to showoff our SQL skills because however it displays in the ascending order.
```
SELECT * FROM products
ORDER BY price DESC;
```
___
```
SELECT * FROM products
ORDER BY inventory DESC, price ASC;
```
This command sorts the results by the _inventory_ colum. If the _inventory_ column has any values that are same, then it sorts them by _price_.

## LIMIT & OFFSET keywords
```
SELECT * FROM products
ORDER BY id
LIMIT 10;
```
This command only displays 10 rows instead of displaying every row in the table.
* This is actually useful beacuse in production we might have thousands or even millions of rows. So displaying all of it might not be possible.
___
```
SELECT * FROM products
ORDER BY id
LIMIT 10 OFFFSET 5;
```
THis command also displays only 10 rows but after skipping the first 5.
* For example, if the above query without the LIMIT and OFFSET keywords return a total of 20 records, 
* Then, by using LIMIT of 10 and OFFSET of 5, the first 10 records after skipping the first 5 is displayed.

## Adding a new entry
```
INSERT INTO products (name, price, serving_time) VALUES ('shawerma', 120, 7);
```
This command adds a new entry to the  _products_ table.
* The parantheses after the table name contains the name of the columns seperated by commas, for which you'd like to provide the value 
* And in the parantheses after VALUES, you provide the actual values in the **same order** of the columns you mentioned in the first parantheses.
* You can also multiple entries at the same time by spcifying another set of values in parantheses seperated by a comma
```
INSERT INTO products (name, price, serving_time) 
VALUES ('Nachos', 70, 6), ('Wedges', 65, 6) returning *;
```
### Returning the new entry
```
INSERT INTO products (name, price, serving_time) VALUES ('shawerma', 120, 7) returning *;
```
With the **RETURNING** keyword, we display the row(s) that have been newly inserted. 
* Since we specify an * here, it returns all the columns. We can also mention the column names we'd like to see for the new entry like we did with SELECT.

## Deleting entries 
```
DELETE FROM products 
WHERE id = 16 RETURNING *;
```
This command is used to delete an entry from the table _products_ whose _id_ is 16. It also returns the row that is being deleted.

## Updating entries in a table
```
UPDATE products
SET serving_time = 9, price = 110
WHERE name = 'Pizza'
RETURNING *;
```
What this command does is, it grabs the entry which has the name 'Pizza' and updates the serving_time and the price to the given values. 

## Joining tables
Read about the **JOIN** command [here](https://www.postgresqltutorial.com/postgresql-joins/).
### Key Points
* If you have column with the same name in the two tables you are joining, then we need to reference that column name by specifying a `.` after the table name. For example,
    ```
    select posts.id, users.id AS user_id, email from posts LEFT JOIN users ON user_id = users.id;
    ```
    * Here, we reference the _id_ column of the _users_ table as _users.id_ and that of _posts_ table as _posts.id_.
    * However this is not limited to JOIN command, we can use it other times aswell
* LEFT JOIN is actually LEFT OUTER JOIN
* When using the COUNT() function, if you use COUNT(*), it also counts the null entries, so to avoid that we need to give a column name instead of *