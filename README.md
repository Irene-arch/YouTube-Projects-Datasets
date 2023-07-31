# YouTube-Projects-Datasets
This repo contains all the datasets for my YouTube videos. Updated as more videos are uploaded.

1. **Excel Call Center Dashboard** - [YouTube](https://youtu.be/VJVuDbIRWAc) | [Dataset Link](https://github.com/Irene-arch/YouTube-Projects-Datasets/blob/main/Human%20Resources.csv)
2. **SQL & PowerBI Project** - [YouTube](https://youtu.be/PzyZI9uLXvY) | [Dataset Link](https://github.com/Irene-arch/YouTube-Projects-Datasets/blob/main/Human%20Resources.csv)
3. **Data Cleaning in Python (FIFA21)** - [YouTube](https://youtu.be/7mYbrpfAU6k) | [Dataset Link](https://github.com/Irene-arch/YouTube-Projects-Datasets/blob/main/fifa21%20raw%20data%20v2.csv)
4. **Customer Behaviour Analysis Using SQL** - [YouTube](https://youtu.be/lHR1_j8DYFA) | [Dataset Link](https://github.com/Irene-arch/YouTube-Projects-Datasets/blob/main/Dannys%20Diner%20Template.sql)


<details close>
  <summary> <b> SQL CHEATSHEET - Beginner to Advanced </b> </summary>
  <br>
  
**Create a table**
```sql
CREATE TABLE table_name(
  column_a datatype,
  column_b datatype,
  column_c datatype
);
```
**RENAME A COLUMN**

```sql
ALTER TABLE table_name
RENAME COLUMN old_name TO new_name;
```

**Extract the last two characters from a string**
  
  ```sql
RIGHT('sports',2)
```

**Join 2 columns using a space**

```sql
SELECT concat(first_name, ' ', last_name) AS full_name
FROM employees;
```

## PRIMARY KEYS

- Need to be defined on columns that don't accept duplicate or null values.
- Are time-invariant, meaning that they must hold for the current data in the table – but also for any future data that the table might hold. Choose columns where values will always be unique and not null.
  
### Specifying Primary Keys

```sql
CREATE TABLE products (
product_no integer UNIQUE NOT NULL,
name text,
price numeric
);
```

```sql
CREATE TABLE products (
product_no integer PRIMARY KEY,
name text,
price numeric
);
```

If you want to designate more than one column as the primary key. That's still only one primary key, it is just formed by the combination of two columns. Primary keys consist of as few columns as possible.

```sql
CREATE TABLE example (
a integer,
b integer,
c integer,
PRIMARY KEY (a, c)
);
```

Adding a primary key to an existing table

```sql
ALTER TABLE table_name
ADD CONSTRAINT some_name PRIMARY KEY (column_name)
```

### SURROGATE KEYS

- They are sort of an artificial primary key.
- They are not based on a native column in your data, but on a column that just exists for the sake of having a primary key. For example 'id' column that auto increments
- They provide a simple non meaningful value that can be used as a primary key to uniquely identify each record in a table.
- If you try to specify an 'id' that already exists, the primary key constraint will prevent you from doing so.

**Adding a surrogate key with serial data type**

```sql
ALTER TABLE cars
ADD COLUMN id serial PRIMARY KEY;
INSERT INTO cars
VALUES ('Volkswagen','Blitz','black');
```

**Another strategy is combining 2 columns into one**
First add a new column and then update the column by concatenating two existing columns.

```sql
ALTER TABLE table_name
ADD COLUMN column_c varchar(256);

UPDATE table_name
SET column_c = CONCAT(column_a, column_b);
ALTER TABLE table_name
ADD CONSTRAINT pk PRIMARY KEY (column_c);
```

  </details>
