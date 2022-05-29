<details><summary>Collapse</summary><p>
  
``` PYTHON
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
  
  
# Null value: the value that is unknown (doesnt mean equal 0). It is ok to be empty
# CRUD: stands for Create, read, update and delete

```
</p>
</details>
