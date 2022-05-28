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
  
  
  
  

# Null value: the value that is unknown (doesnt mean equal 0). It is ok to be empty

```
