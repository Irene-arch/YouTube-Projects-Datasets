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

### FOREIGN KEYS

- Designated columns that point to a primary key of another table.
- It is not necessarily an actual key, because duplicates and "NULL" values are allowed.

**Restrictions of Foreign Keys**

- The domain and the data type must be the same as one of the primary keys.
- Each value of the foreign key must exist in the primary key of the other table. This is the actual foreign key constraint, also called "referential integrity"

First, we create a "manufacturers" table with a primary key called "name". Then we create a table "cars", that also has a primary key, called "model". 

```sql
CREATE TABLE manufacturers (
name varchar(255) PRIMARY KEY);

INSERT INTO manufacturers
VALUES ('Ford'), ('VW'), ('GM');

CREATE TABLE cars (
model varchar(255) PRIMARY KEY,
manufacturer_name varchar(255) REFERENCES manufacturers (name));
```

As each car is produced by a certain manufacturer, it makes sense to also add a foreign key to this table. We do that by writing the "REFERENCES" keyword, followed by the referenced table and its primary key in brackets. From now on, only cars with valid and existing manufacturers may be entered into that table. Trying to enter models with manufacturers that are not yet stored in the "manufacturers" table won't be possible, thanks to the foreign key constraint.

**Specifying foreign keys to existing tables**

The syntax for adding foreign keys to existing tables is the same as the one for adding primary keys and unique constraints.

```sql
ALTER TABLE a
ADD CONSTRAINT a_fkey FOREIGN KEY (b_id) REFERENCES b (id);
```

**Update columns of a table based on values in another table**

```sql
UPDATE table_a
SET column_to_update = table_b.column_to_update_from
FROM table_b
WHERE condition1 AND condition2 AND ...;
```

### REFERENTIAL INTEGRITY

- A record referencing another record in another table must always refer to an existing record.
- A record referencing another table must refer to an existing record in that table.
- It is a constrant specified between two tables and is enforced through foreign keys

Referential integrity can be violated in two ways:
- If a record in table B that is referenced from a record in table A is deleted.
- If a record in table A referencing a non-existing record from table B is inserted.

Trying to do any of these will make SQL throw an error. However you can also specify what you want to be done in case of a violation. For example:
By default, the "ON DELETE NO ACTION" keyword is automatically appended to a foreign key definition.

```sql
CREATE TABLE a (
id integer PRIMARY KEY,
column_a varchar(64),
...,
b_id integer REFERENCES b (id) ON DELETE NO ACTION
);
```

This means that if you try to delete a record in table B which is referenced from table A, the system will throw an error.

There's the "CASCADE" option, which will first allow the deletion of the record in table B, and then will automatically delete all referencing records in table A. So that deletion is cascaded.

```sql
CREATE TABLE a (
id integer PRIMARY KEY,
column_a varchar(64),
...,
b_id integer REFERENCES b (id) ON DELETE CASCADE
);
```

**Other options**

ON DELETE...
1. ...NO ACTION: Throw an error
2. ...CASCADE: Delete all referencing records
3. ...RESTRICT: Throw an error
4. ...SET NULL: Set the referencing column to NULL. It will set the value of the foreign key for this record to "NULL"
5. ...SET DEFAULT: Set the referencing column to its default value. Only works if you have specified a default value for a column. It automatically changes the referencing column to a certain default value if the referenced record is deleted.

Altering a key constraint doesn't work with ALTER COLUMN. Instead, you have to DROP the key constraint and then ADD a new one with a different ON DELETE behavior.
For deleting constraints, though, you need to know their name. This information is also stored in information_schema.

  </details>
