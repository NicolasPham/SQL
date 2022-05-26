``` PYTHON
# Create table
  CREATE TABLE <table_name>
                 (
                 name VARCHAR(100) NOT NULL DEFAULT 'unnamed',
                 address VARCHAR(10) NOT NULL,
                 age INT NOT NULL DEFAULT 0
                 );

# Insert / adding data into the table
  INSERT INTO <table_name>(column1, column2, etc)
  VALUES (variable 1a, variable2a, .etc)
          ,(variable1b, variabl2b, .etc)
          ,(variable1c, variable2c, .etc);
  
  SELECT * FROM <table_name>;

# Null value: the value that is unknown (doesnt mean equal 0). It is ok to be empty

```
