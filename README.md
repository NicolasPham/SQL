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
> Concat:  combine data for cleaner output
  concat (<column2>, 'text', <column2>, 'text') : 
     SELECT CONCAT(first_name, ' ', last_name) AS <new_name> FROM <table_name>;
  
     SELECT author_fname as first, author_lname as last, 
     CONCAT (author_fname, author_lname) AS full,
     FROM books;

> Concat_WS
  CONCAT('text', <column1>, <column2>, .etc) : combine all columns and separate them by 'text'

> SUBSTRING
  SELECT SUBSTRING('STRING',#1, #2): return the character from #1 to #2, start with 1
    SELECT SUBSTING ('Hello World', 7): World
    SELECT SUBSTRING('Hello World', -3): rld (how many last characters we want to get)



# Null value: the value that is unknown (doesnt mean equal 0). It is ok to be empty
# CRUD: stands for Create, read, update and delete

```
</p>
</details>
