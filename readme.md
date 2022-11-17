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
    WHERE customer_id = 10;
```


## Updating Tables

### Use Case
Used to update existing tables to add new information. 

### Example Syntax
```
    INSERT INTO customers (
        first_name,
        last_name,
        email,
        age,
        gender,
        address
    ) VALUES (
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

In this example, the JOIN operator is used to return customer names and total items purchased from the customer table and invoice table where the customer id matches the customer id on the invoice.

```
    SELECT 
        c.first_name || ' ' || c.last_name AS name,
        i.total, 
    FROM customers c
    JOIN invoices i 
    ON c.customer_id = i.customer_id;
```

In the example below, a **self JOIN** is used to extract information in different ways from the same table. The query below returns the employee name along with the manager's name who they report to by using an INNER JOIN where the employee_id is equal to the 'reports_to' employee_id number.

```
    SELECT 
        CONCAT(e.first_name, ' ', e.last_name) employee,
        CONCAT(m.first_name, ' ', m.last_name) manager
    FROM 
        employee e
    INNER JOIN employee m ON m.employee_id = e.reports_to
    ORDER BY manager;
```

## Grouping and Ordering (GROUP BY, ORDER BY)

### Use Case
GROUP BY is used to group output under certain conditions such as items purchased by customer. 

ORDER BY is used to order the output either in ascending numerical order or alphabetical order. Using 'DESC' will make change this to descending or reverse alphabetical order.
 

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

Aggregate functions are used frequently to get numbers from the tables - that could be adding things with SUM, counting rows with COUNT, or finding summary statistics using MIN, MAX, or AVG. 

### Example Syntax 
  
###### SUM  

This example shows the customers who have spent the most. 

```
    SELECT 
        CONCAT(c.first_name, ' ', c.last_name) customer_name, 
        SUM(i.total) total
    FROM customer c
    JOIN invoice i 
    ON c.customer_id = i.customer_id
    GROUP BY customer_name
    ORDER BY total DESC;
```  

###### COUNT  

This example returns the number of unique customers in the table by counting the distinct customer_id values. 

```
SELECT DISTINCT COUNT(customer_id)
FROM customer;
```
###### AVG  
 
This example returns the average amount spent by each customer on the invoices in the database.  

```
    SELECT 
        CONCAT(c.first_name, ' ', c.last_name) customer_name, 
        AVG(i.total) average_total
    FROM customer c
    JOIN invoice i 
    ON c.customer_id = i.customer_id
    GROUP BY customer_name
    ORDER BY average_total DESC;
```
  
###### MIN 

This example returns the least amount spent by each customer according to the invoices in the database. 

```
    SELECT 
        CONCAT(c.first_name, ' ', c.last_name) customer_name, 
        MIN(i.total) min_total
    FROM customer c
    JOIN invoice i 
    ON c.customer_id = i.customer_id
    GROUP BY customer_name
    ORDER BY min_total DESC;
```  

## Using CASE

### Use Case  
Case is used to create additional columns based on specified conditions in the data. It can be used to label data or for 'binning' - to create categories for numerical data. 

### Example Syntax  

In the example below, a new column will appear in the query output which categorizes the invoices into 'small,' 'medium,' 'large,' and 'x-large' order sizes based on the total.  
  
```
    SELECT *, 
    CASE WHEN total > 10 THEN 'x-large'
         WHEN total > 6 THEN 'large'
         WHEN total > 3 THEN 'medium'
         ELSE 'small' END AS order_size
    FROM invoice;
```


