# MySQL Useful Queries

## Query 1: Database Schema Overview - Extracting Table and Column Information

```sql
SELECT
    GROUP_CONCAT(
        CONCAT(
            'Table Name: ', c.TABLE_NAME,
            '; Column Name: ', c.COLUMN_NAME,
            '; Data Type: ', c.DATA_TYPE,
            IF(c.COLUMN_KEY = 'PRI', '; Primary Key', ''),
            IF(kcu.REFERENCED_TABLE_NAME IS NOT NULL,
               CONCAT('; Foreign Key to ', kcu.REFERENCED_TABLE_NAME, '(', kcu.REFERENCED_COLUMN_NAME, ')'),
               ''
            )
        ) SEPARATOR '\n'
    ) AS Table_Column_Information
FROM
    information_schema.COLUMNS c
LEFT JOIN
    information_schema.KEY_COLUMN_USAGE kcu
    ON c.TABLE_SCHEMA = kcu.TABLE_SCHEMA
    AND c.TABLE_NAME = kcu.TABLE_NAME
    AND c.COLUMN_NAME = kcu.COLUMN_NAME
    AND kcu.REFERENCED_TABLE_NAME IS NOT NULL
WHERE
    c.TABLE_SCHEMA = 'your_database';
```

### Description:
This query retrieves detailed column information from the 'columns' table in the 'information_schema' database, focusing on a specified database (replace 'your_database' with the actual database name). The output is a well-formatted, comma-separated list of concatenated strings. Each string contains information about a column in a table, including the table name, column name, data type, and whether it is a primary key. The result is labeled with the alias 'Table_Column_Information'.

---

## Query 2: List all Tables in a Database

```sql
SHOW TABLES FROM your_database;
```

### Description:
This query returns a simple list of all tables in the specified database, providing a quick overview of the available tables.

---

## Query 3: Show Table Structure

```sql
DESCRIBE your_table;
```

Replace 'your_table' with the actual table name. This query offers a comprehensive view of the specified table's structure, detailing column names, data types, and other relevant attributes.

---

## Query 4: Retrieve Unique Values from a Column

```sql
SELECT DISTINCT your_column FROM your_table;
```

Replace 'your_column' with the actual column name and 'your_table' with the table name. This query efficiently fetches unique values from a specific column in a table.

---

## Query 5: Count Rows in a Table

```sql
SELECT COUNT(*) AS Row_Count FROM your_table;
```

Replace 'your_table' with the actual table name. This query provides a quick count of the total number of rows in the specified table, aiding in basic data analysis.

---

## Query 6: Find Duplicate Rows in a Table

```sql
SELECT your_columns, COUNT(*) AS Duplicate_Count
FROM your_table
GROUP BY your_columns
HAVING COUNT(*) > 1;
```

Replace 'your_columns' with the actual column names and 'your_table' with the table name. This query efficiently identifies and lists duplicate rows based on specified columns, facilitating data quality checks.

---

## Query 7: Update Column Values

```sql
UPDATE your_table
SET your_column = 'new_value'
WHERE your_condition;
```

Replace 'your_table' with the table name, 'your_column' with the column to be updated, 'new_value' with the desired value, and 'your_condition' with the condition for the update. This query allows for easy updating of specific column values based on defined conditions.
