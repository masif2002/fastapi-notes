# Database Management System
* Database is a collection of organized data that can be easily accessed and managed. 
* But, we don't interact with databases directly. Instead, we have a Database Management System (DBMS) which is going to act as the moderator bwetween us and the database.
* When we need to perform an operation on a database, we are going to send that request to the database management system, which is actually going to perform tha operation and then is going to send the result back to us.

* There are two types of databses
    * **Relational Database** - MYSQL, POSTGRESQL, ORACLE, SQL SERVER
    * **NoSQL Database** - MongoDB, DynamoDB, ORACLE, SQL SERVER


* Structured Query Language (SQL) is the language used to communicate with DBMS.
    * When we need to perform an operation, we are going to send a specific SQL command to the database management system which is going to perfrom the operation in the database and return the results.

## Basics of Relational Database
* A table represents a subject or an event in an application
    * A table is made up of rows and columns
    * Each column represents a different attribute
    * Each row represents a different entry in the table

### Datatypes in Postgres
* Int, decimal, precision - for numeric datatypes
    * We also have _smallint_ and _bigint_. The different between them is the number of bits they can accomodate. 
    * We also have something called a _serial_ which serialises the entries, ie- numbers the entries in the table from 1 till n. It can be useful for an _id_ column. However, it is just a regular integer but with this property of incrementing the integer for every new entry.
* varchar (varying character), text - for character datatypes
* boolean
* array

There is also a datatype called _timestamp with timezone_ which when given a value of **NOW()** in the _default_ field, stores the timestamp with the timezone when the entry was created.

### Primary Key
When we specify a table, we need to specify something called a primary key.
* A **Primary key** is basically a column with which we uniquely identify each entry in a table.
* A table can have just one primary key.
* We usually have a column called _id_ which we use for a primary key. But it necessarily not needs to be an _id_ column. It could be any other column that is **unique** which you feel is suitable to set it as the primary key for the table.

## Composite key

A composite key is nothing more than a primary key that spans multiple columns.
* We've only worked with one column primary keys, but we can have it actually cover more than one column.
* A primary key must be unique. So, when it spans acoss multiple columns, say 2 columns, then both of those columns must be unique together, not seperately.
    * So that means, we cannot have both of the same column values in two different rows in that table.
```
      |   A    |    B    |   
      |________|_________|
 1.   |   12   |    4    |
 2.   |   28   |    9    |
 3.   |   55   |    2    |
 4.   |   12   |    9    |
 5.   |   18   |    4    |
 6.   |   55   |    2    |
```
* In the above example, when we have composite primary key for column A and B, row 6 would be a conflict because, there is already a pair of the same value in row 3.
### Unique constraint 
A **UNIQUE** constraint can be applied to any column to make sure the entries in that column is always unique.
### Null Constraint
* By default, when adding a new entry to a database, any column can be left blank. When a column is left blank, it has a null value.
* But when creating a column, we can add a **NOT NULL** constraint which ensures that the column is never left blank. 