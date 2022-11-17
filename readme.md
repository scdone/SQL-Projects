# SQL Queries I Have Experience Writing:


## Creating Tables

### Use Case
Used to create new tables in databases. 

In the below example, using 'IF NOT EXISTS' ensures no duplicate tables will be created by accident. 

### Example Syntax

```
    CREATE TABLE IF NOT EXISTS customers (
        customer_id SERIAL PRIMARY KEY,
        first_name VARCHAR(25),
        last_name VARCHAR(25),
        email VARCHAR(50),
        age INT(3),
        gender VARCHAR(25),
        address VARCHAR(50)
        );
```

## Deleting Information

### Use Case
Used to delete entire tables or deleting specific information from tables in the database when necessary. Note: If the table has been created or altered with 'ON DELETE CASCADE' then any foreign keys in other tables will also be deleted when the table is dropped. 

### Example Syntax

##### Delete an entire table:

```
    DROP TABLE customers;
```
##### Delete information based on a condition:

```
    DELETE FROM customers
    WHERE customer_id = 10
```


## Updating Tables

### Use Case
Used to update existing tables to add new information. 

### Example Syntax
```
    INSERT INTO customers (
        customer_id,
        first_name,
        last_name,
        email,
        age,
        gender,
        address
    ) VALUES (
        5,
        'Jessica',
        'Smith',
        'jsmith22@gmail.com',
        35,
        'female',
        '526 E 10th ST'
    );
```

## JOIN Statements

### Use Case

Used to get information from more than one table. Joins are usually performed on foreign keys from other tables such as customer_id. 

### Example Syntax

```
    SELECT c.first_name || ' ' || c.last_name AS name,
            i.total, 
    FROM customers c
    JOIN invoices i 
    ON c.customer_id = i.customer_id;
```

## Grouping and Ordering (GROUP BY, ORDER BY)

### Use Case
Group by is used to group output under certain conditions such as items purchased by customer. 

Order by is used to order the output either in ascending numerical order or alphabetical order. Using 'DESC' will make change this to descending or reverse alphabetical order.
 

### Example Syntax
In the example syntax below, GROUP BY and ORDER BY are used to find the total items purchased by each customer with an ordered output of customers with the highest items purchased descending.
```
    SELECT c.first_name || ' ' || c.last_name AS customer_name,
                SUM(i.total) AS total
    FROM customer c
    JOIN invoice i
    ON c.customer_id = i.customer_id
    GROUP BY customer_name
    ORDER BY total DESC;
```


## LIKE operator

### Use Case

Using the LIKE operator allows you to search for a substring within a string in a certain column. Percentage symbols act as unknown characters and can be used before, after, or both in searching for a substring within text. 

### Example Syntax
The example below will return all employees who have 'Manager' in their title.

```
SELECT * FROM employees
WHERE title LIKE '%Manager%';
```


## Aggregate Functions (SUM, COUNT, MIN/MAX, AVG)

### Use Case

### Example Syntax


## Using CASE

### Use Case

### Example Syntax

