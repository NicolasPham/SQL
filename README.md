TABLES:
<details><summary>Collapse</summary><p>
 
``` sql
> Table rows and columns are referred to as records and fields
> Good table manners:
	>> table names should be lowercase, with no spaces, plural
	>> field names should be lowercase, with no spaces, singular, be different from other field names
	>> unique identifier (Key): 
# Create table
  CREATE TABLE <table_name>
                 (
                 id VARCHAR(16) NOT NULL AUTO_INCREMENT
                 name VARCHAR(100) NOT NULL DEFAULT 'unnamed',
                 address VARCHAR(10) NOT NULL,
                 age INT NOT NULL DEFAULT 0
                 PRIMARY KEY (id)
                 );
# Data relationships:
   Primary key: keep the row unique
   Foreign key: are references to another table from one table
   On delete cascade: delete values from foreign table if parents values deleted
     > CREATE table customers(
	                      customer_id INT AUTO_INCREMENT PRIMARY KEY,
	                      ....
	                      );
     > CREATE table orders(
	                   ....
	                   customer_id INT,
	                   FOREIGN KEY(customer_id) REFERENCES customers(customer_id)
	                   ON DELETE CASCADE
			-- delete freely value from both values parents and references
			   );
	
# Insert / adding data into the table
  INSERT INTO <table_name>(column1, column2, etc)
  VALUES (variable 1a, variable2a, .etc)
          ,(variable1b, variabl2b, .etc)
          ,(variable1c, variable2c, .etc);
  
  SELECT <column_name1> as <....>, <column_name2> FROM <table_name>
          WHERE <condition>;
  
  # Update the data: make sure to target the right data before updating
  UPDATE <table_name> SET <column_name> = ''  
          WHERE <condition>;
  
  DELETE FROM <table_name> WHERE <column_name> = <condition>;
  TRUNCATE [table] table_name;
	
# Add | Drop column to table
   ALTER TABLE table_name
	ADD [COLUMN] column_name column_definition [FIRST | AFTER existing_column];
   ALTER TABLE table_name
	DROP COLUMN column_name
```
</p>
</details>
STRING FUNCTIONS:
<details><summary>Collapse</summary><p>
  
``` sql
> CONCAT:  combine data for cleaner output
  >> concat (<column2>, 'text', <column2>, 'text') : 
     >>> SELECT CONCAT(first_name, ' ', last_name) AS <new_name> FROM <table_name>;
  
     SELECT author_fname as first, author_lname as last, 
     CONCAT (author_fname, author_lname) AS full,
     FROM books;

> Concat_WS
  >> CONCAT('text', <column1>, <column2>, .etc) : combine all columns and separate them by 'text'

> SUBSTRING
  >> SELECT SUBSTRING('STRING',#1, #2): return the character from #1 to #2, start with 1
    >>> SELECT SUBSTING ('Hello World', 7): World
    >>> SELECT SUBSTRING('Hello World', -3): rld (how many last characters we want to get)

> REPLACE: replace parts of the string
  >> SELECT REPLACE ('Hello World', 'Hell', '1234'): '1234o World'
  >> SELECT REPLACE (title, 'e', '3') FROM books;

> CHAR_LENGTH(): count how many characters in the string
  >> SELECT CHAR_LENGTH('Hello World'): 11
  
> UPPER and LOWER: Change a string case
```
</p>
</details>  
REFINING SELECTION
<details><summary>Collapse</summary><p>
  
``` sql
  > VIEW
    >> CREATE VIEW employee_hire_years AS SELECT ....
  > DISTINCT: return only distinct (different) values
    >> SELECT DISTINCT author_lname FROM books;
    >> SELECT DISTINCT author_fname, author_lname FROM books;
  
  > ORDER BY: (sorting our values)
    >> SELECT author_lname FROM books ORDER BY author_lname; (ascending by default)
    >> SELECT author_lname FROM books ORDER BY author_lname DESC;
    >> SELECT title, author_fname, author_lname FROM books ORDER BY 2: 2 is author_fname
  
  > LIMIT: state how many rows we want to get
    >> SELECT title FROM books ORDER BY released_year DESC LIMIT 2,5; take 5 books that have the most sells "after" 2
    
  > LIKE : better search
    >> WHERE author_fname LIKE '%da%' : where author first name has the letters "da" in it
      >>> % ... % is called wildcard
    >> WHERE author_fname LIKE 'da%' : where author first name start "da"
    >> WHERE stock_quantity LIKE '___' : 3 underscors equal to 3 digits exactly
    >> WHERE title LIKE '%\%%': title contains '%' in it
    >> SELECT title FROM books WHERE title LIKE '%\_%': title has '_' in it
  
  > HAVING:  another word of WHERE but can be put at the end of the querry
  > IN (value1, value2)
  > BETWEEN value1 and value2 
  > CAST(): convert anything from one data type to another
  	>> CAST(purchase_price AS FLOAT64)
  > COALESCE(): return non-null value in a list
  	>> COALESCE(product, product_code) AS product_info : check product column first, if it is null then check product_code
  
```
</p>
</details>  
AGGREGATE FUNCTIONS;
<details><summary>Collapse</summary><p>
  
``` sql
  COUNT: return the number of rows that matches the specific criteria
    > SELECT COUNT(DISTINCT author_fname, author_lname) FROM books;
    > SELECT COUNT(title) FROM books WHERE tile LIKE '%the%';
    > SELECT COUNT(*) AS total_records FROM people: count records in the table
  
  GROUP BY: summarizes or aggregates identical data into single rows
    > SELECT author_lname, COUNT(*) FROM books GROUP BY author_lname;
    > SELECT CONCAT('In ', released_year, ', ', COUNT(*), ' book(s) released') AS yell FROM books GROUP BY released_year ORDER BY released_year;
   
  MIN nad MAX:
    > SELECT MIN(released_year) FROM books;
      >> SELECT CONCAT(author_fname, ' ', author_lname) as full_name,
		    pages FROM BOOKS WHERE pages = (SELECT MAX(pages) from books);
		   
   SUM:
      > SELECT first_name, last_name, order_date, SUM(amount) AS total_amount
        FROM customers JOIN orders ON customers.id = orders.customer_id
	GROUP BY orders.customer_id
	ORDER BY total_amount DESC;
  IFNULL:
     > SELECT first_name, 
	last_name, 
	IFNULL (SUM(amount),0) as total_spent
	FROM customers LEFT JOIN orders ON customers.id = orders.customer_id
	GROUP BY customers.id
	ORDER BY total_spent DESC;
   IF function:
	-- For more than 2 cases:
	CASE
	   WHEN AVG(grade) >=75 THEN 'PASSING'
	   WHEN AVG(grade) IS NULL THEN 'FAILING'
	   ELSE 'FAILLING'
   	END AS passing_status
	
	-- For 2 cases: 
	IF(AVG(grade) >= 75, 'PASSING', 'FAILLING') AS passing_status
	
# Null value: the value that is unknown (doesnt mean equal 0). It is ok to be empty
# CRUD: stands for Create, read, update and delete
# Data Type:
  > CHAR(), VARCHAR()
  > INT, DECIMAL(5,2), FLOAT, DOUBLE
  > DATE, TIME, DATETIME(), CURDATE(), CURTIME(), NOW()
    >> https://dev.mysql.com/doc/refman/8.0/en/date-and-time-functions.html
    >> DATE_FORMAT(date, format): https://dev.mysql.com/doc/refman/8.0/en/date-and-time-functions.html#function_date-format
  > Date Math:
    >> DATEDIFF(expr1, expr2): the number of days
    >> DATE_ADD(date, interval, expr unit) / DATE_SUB(date, interval, expr unit): 
    >> TIMESTAMP
       >>> CREATE TABLE comments(
                                  content VARCHAR(100), 
                                  changed_at TIMESTAMP DEFAULT NOW() ON UPDATE CURRENT_TIMESTAMP
                                );

```
</p>
</details>	
JOINING TABLE
<details><summary>Collapse</summary><p>
  
``` sql
   # Cross join: Give all the possible combinations between 2 tables
      SELECT * FROM customers, orders; 
   # Inner join: Give the only matching values between 2 tables
      SELECT first_name, last_name, order_date, amount 
      FROM customers, orders 
      WHERE / (ON) customers.id = orders.customer_id;

      SELECT irst_name, last_name, order_date, amount FROM customers
	(INNER) JOIN orders ON customer.id = orders.customer_id;
	   # Inner is optional
   # Left join: Keep all the values in the left table, and match whatever values of the right table to the left table
      SELECT irst_name, last_name, order_date, amount FROM customers
	LEFT JOIN orders ON customer.id = orders.customer_id;	
   
      
	
	
```
</p>
</details>	

LOGICAL OPERATORS:
	
``` SQL
> %: Called Modulo, return the remainder when one number is divided by another (same as MOD in spreadsheer)
> Not Equal !=
> NOT LIKE / LIKE
> AND &&
> -- : comment not, a querry
	
```

NODEJS using MySQL
<details>

```sql


```
</details>

