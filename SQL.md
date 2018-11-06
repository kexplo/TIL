## Using String as Primary Key

reference: https://stackoverflow.com/questions/3455297/mysql-using-string-as-primary-key

> There's nothing wrong with using a CHAR or VARCHAR as a primary key.
>
> Sure it'll take up a little more space than an INT in many cases, but there are many cases where it is the most logical choice and may even reduce the number of columns you need, improving efficiency, by avoiding the need to have a separate ID field.


## What is the difference between a primary key and a index key

reference: https://stackoverflow.com/questions/5374908/what-is-the-difference-between-a-primary-key-and-a-index-key

> A primary key is a special kind of index in that:
>
> - there can be only one;
> - it cannot be nullable; and
> - it must be unique.



## MySQL string sorting

reference: https://stackoverflow.com/a/8557307


### Alpha Numeric Sorting in MySQL

Given input

```
1A 1a 10A 9B 21C 1C 1D
```

Expected output

```
1A 1C 1D 1a 9B 10A 21C
```

Query

```sql
-- Bin Way
-- ===================================
SELECT 
tbl_column, 
BIN(tbl_column) AS binray_not_needed_column
FROM db_table
ORDER BY binray_not_needed_column ASC , tbl_column ASC

-----------------------

-- Cast Way
-- ===================================
SELECT 
tbl_column, 
CAST(tbl_column as SIGNED) AS casted_column
FROM db_table
ORDER BY casted_column ASC , tbl_column ASC
```

### Natural Sorting in MySQL

Given input

```
Table: sorting_test
 -------------------------- -------------
| alphanumeric VARCHAR(75) | integer INT |
 -------------------------- -------------
| test1                    | 1           |
| test12                   | 2           |
| test13                   | 3           |
| test2                    | 4           |
| test3                    | 5           |
 -------------------------- -------------
 ```

Expected Output

```
 -------------------------- -------------
| alphanumeric VARCHAR(75) | integer INT |
 -------------------------- -------------
| test1                    | 1           |
| test2                    | 4           |
| test3                    | 5           |
| test12                   | 2           |
| test13                   | 3           |
 -------------------------- -------------
```

Query

```sql
SELECT alphanumeric, integer
       FROM sorting_test
       ORDER BY LENGTH(alphanumeric), alphanumeric  
```

### Sorting of numeric values mixed with alphanumeric values

Given input

```
2a, 12, 5b, 5a, 10, 11, 1, 4b
```

Expected Output

```
1, 2a, 4b, 5a, 5b, 10, 11, 12
```

Query

```sql
SELECT version
FROM version_sorting
ORDER BY CAST(version AS UNSIGNED), version;
```

## mysqldump

```bash
# dump whole database
$ mysqldump -h <host> -p -u <user> <database> > dump.sql
```

```bash
# dump specific table(s)
$ mysqldump -h <host> -p -u <user> <database> <table> [<table> <table> ...] > dump.sql
```

```bash
# restore
$ mysql -u <user> -p < dump.sql
```