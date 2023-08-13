Step to answer question:
1. Desired result: exp: a table that sumarizes the revenue per platform per year
2. Stored where? :
3. Which columns?
4. Which functions: exp: sum up the revenue per year
5. What filters:

Data type:
  > Date-time: datetime, date, time, datetime2, smalldatetim

Data type precedence:
  > user_defined > datetime > date > float > decimal > int > bit > nvarchar > varchar > binary
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
  Add column:
	ALTER TABLE table_name ADD column_name data_type
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

# Save query into
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
	>> %: match zero, one, or many character
	>> _: match a single character
    >> WHERE author_fname LIKE '%da%' : where author first name has the letters "da" in it
      >>> % ... % is called wildcard
    >> WHERE author_fname LIKE 'da%' : where author first name start "da"
    >> WHERE stock_quantity LIKE '___' : 3 underscors equal to 3 digits exactly
    >> WHERE title LIKE '%\%%': title contains '%' in it
    >> SELECT title FROM books WHERE title LIKE '%\_%': title has '_' in it
  
  > HAVING:  another word of WHERE but can be put at the end of the querry
  > IN (value1, value2)
  > BETWEEN value1 and value2
  > AND, OR : use parentheses to make it clear which criteria should be checked first
  > CAST(): convert anything from one data type to another
  	>> CAST(purchase_price AS FLOAT64)
  > CONVERT() : same as CAST()
	>> CONVERT(data_type [length], expression [,style])
	  >>> CONVERT(int, 3.14) AS decimal_to_int
	  >>> CONVERT(varchar(20), GETDATE(), 104) AS date_to_string: 104 is a code for day.month.year
  > COALESCE(): return non-null value in a list
  	>> COALESCE(product, product_code) AS product_info : check product column first, if it is null then check product_code
  > NULL
    > IS NULL: WHERE birthdat IS NULL : return the table where birthdate has missing values
    > IS NOT NULL: WHERE birthdate IS NOT NULL: return the table where birthdate is not null

  > CASE: create a column
    > CASE WHEN x = 1 THEN 'a'
    	WHEN x = 1 THEN 'b'
	ELSE 'c' END AS new_column
   > SELECT COUNT(CASE WHEN home_goal > away_goal AND hometeam_id = 8650 THEN id) END AS home_wins
     FROM matches GROUP BY season

   > Semi-joint:
SELECT DISTINCT name
FROM languages
WHERE code IN
  (SELECT code FROM countries
  WHERE region LIKE 'Middle East')
ORDER BY name;

   > Anti-joint:
SELECT code, name
FROM countries
WHERE continent LIKE 'Oc%'
AND code NOT IN
	(SELECT code FROM currencies)
  
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
  ROUND:
    > SELECT ROUND(AVG(budget), 2) FROM films: 412239.28
    > SELECT ROUND(AVG(budget), -3) FROM films: 412000
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

	SELECT prim_minister, president
	  FROM prime_ministers as p1
	  CROSS JOIN presidents as p2
	  WHERE p1.continent IN ('North America', 'Oceania');

   # Inner join: Give the only matching values between 2 tables
      SELECT first_name, last_name, order_date, amount 
      FROM customers, orders 
      WHERE / (ON) customers.id = orders.customer_id;

      SELECT irst_name, last_name, order_date, amount FROM customers
	(INNER) JOIN orders ON customer.id = orders.customer_id;
	   # Inner is optional

      SELECT company.name 
      FROM company
	INNER JOIN fortune500 
	ON company.ticker=fortune500.ticker;

   # Left join: Keep all the values in the left table, and match whatever values of the right table to the left table
      SELECT irst_name, last_name, order_date, amount FROM customers
	LEFT JOIN orders ON customer.id = orders.customer_id;	


  # UNION
SELECT code, year
FROM economies
UNION ALL
SELECT country_code as code, year
FROM populations
ORDER BY code, year

SELECT c1.name
  FROM cities AS c1
  WHERE country_code IN
(
    SELECT e.code
    FROM economies as e  
    UNION
    SELECT c2.code
    FROM currencies as c2
    EXCEPT
    SELECT p.country_code
    FROM populations as p
);
      
	# Subquery
-- Select fields
SELECT name, continent, inflation_rate
  -- From countries
  FROM countries
	-- Join to economies
	INNER JOIN economies
	-- Match on code
	ON countries.code = economies.code
  -- Where year is 2015
  WHERE year = 2015
    -- And inflation rate in subquery (alias as subquery)
    AND inflation_rate IN (
        SELECT MAX(inflation_rate) AS max_inf
        FROM (
             SELECT name, continent, inflation_rate
             FROM countries
             INNER JOIN economies
             ON countries.code = economies.code
             WHERE year = 2015) AS subquery
      -- Group by continent
        GROUP BY continent);
	
```
</p>
</details>	

LOGICAL OPERATORS:
	
``` SQL
> %: Called Modulo, return the remainder when one number is divided by another (same as MOD in spreadsheet)
> Not Equal !=
> NOT LIKE / LIKE
> AND &&
> OR 
> -- : comment not, a querry
	
```

