<details><summary>Collapse</summary><p>
  
``` SQL
# Create table
  CREATE TABLE <table_name>
                 (
                 id VARCHAR(16) NOT NULL AUTO_INCREMENT
                 name VARCHAR(100) NOT NULL DEFAULT 'unnamed',
                 address VARCHAR(10) NOT NULL,
                 age INT NOT NULL DEFAULT 0
                 PRIMARY KEY (id)
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
  
# String functions:
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
  >> SELECT REAPLCE ('Hello World', 'Hell', '1234'): '1234o World'
  >> SELECT REPALCE (title, 'e', '3') FROM books;

> CHAR_LENGTH(): count how many characters in the string
  >> SELECT CHAR_LENGTH('Hello World'): 11
  
> UPPER and LOWER: Change a string case

# Refining the selection
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
  
# Aggregate Functions;
  COUNT
    > SELECT COUNT(DISTINCT author_fname, author_lname) FROM books;
    > SELECT COUNT(title) FROM books WHERE tile LIKE '%the%';
  
  GROUP BY: summarizes or aggregates identical data into single rows
    > SELECT author_lname, COUNT(*) FROM books GROUP BY author_lname;
    > SELECT CONCAT('In ', released_year, ', ', COUNT(*), ' book(s) released') AS yell FROM books GROUP BY released_year ORDER BY released_year;
   
  MIN nad MAX:
    > SELECT MIN(released_year) FROM books;
      >> SELECT CONCAT(author_fname, ' ', author_lname) as full_name,
		    pages FROM BOOKS WHERE pages = (SELECT MAX(pages) from books);
  
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

	
# 1 : Many / Many : Many section:
   # Data relationships:
	Primary key: keep the row unique
	Foreign key: are references to another table from one table
	> CREATE table customers(
	                        customer_id INT AUTO_INCREMENT PRIMARY KEY,
	                        ....
	                        );
	> CREATE table orders(
	                     ....
	                     customer_id INT,
	                     FOREIGN KEY(customer_id) REFERENCES customers(customer_id)
	                     );
   # Cross join: 
      SELECT * FROM customers, orders;
   # Inner join:
      SELECT first_name, last_name, order_date, amount 
      FROM customers, orders 
      WHERE customers.id = orders.customer_id;

      SELECT irst_name, last_name, order_date, amount FROM customers
	JOIN orders ON customer.id = orders.customer_id;
   
      
	
	
#LOGICAL OPERATORS:
> Not Equal !=
> NOT LIKE / LIKE
> AND &&
> -- : comment not, a querry
	
```
</p>
</details>
