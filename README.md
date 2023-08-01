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

## DATABASE DESIGN

### How should we organise and manage data?

1. Schemas - How should my data be logically organised?
2. Normalization - should my data have minimal dependency and redundancy?
3. Views - What joins will be done most often?
4. Access control - should all users of the data have the same level of access?
5. DBMS - how do I pick between all the SQL and NoSQL options?

### Approaches to processing data

They help define the way data is going to flow, be structured and stored.

- OLTP - Online Transaction Processing
- OLAP - Online Analytical Processing

| |OLTP Tasks|OLAP Tasks|
|----|----|----|
|*Purpose*|Support daily transactions|report and analyse data|
|*Design*|application-oriented|subject-oriented|
|*Data*|up to date, operational|consolidated, historical|
|*Size*|snapshot, gigabytes|archive, terabytes|
|*Queries*|simple transactions and frequent updates|complex, aggregate queries & limited updates|
|*Users*|thousands|hundreds|
| |**Example**| |
| |Find the price of a book|Calculate books with the best profit margin|
| |Update latest customer transaction|Find most loyal customers|
| |Keep track of employee hours|Decide employee of the month|
| |Focus on supporting day to day operations|Focus on business decision making|

![image](https://github.com/Irene-arch/YouTube-Projects-Datasets/assets/56026296/04693a87-9bcb-4eff-b704-612b4cf8aeb6)

OLAP and OLTP systems work together; in fact, they need each other. OLTP data is usually stored in an operational database that is pulled and cleaned to create an OLAP data warehouse. Without transactional data, no analyses can be done in the first place. Analyses from OLAP systems are used to inform business practices and day-to-day activity, thereby influencing the OLTP databases.

### Storing Data

1. Structured data - 
Follows a schema
Defined data types & relationships
_e.g., SQL, tables in a relational database _

2. Unstructured data - 
Schemaless
Makes up most of data in the world
e.g., photos, chat logs, MP3

3. Semi-structured data - 
Does not follow larger schema
Self-describing structure
e.g., NoSQL, XML, JSON

### Data warehouses

Optimized for read-only analytics. They combine data from multiple sources and use massively parallel processing for faster queries. In their database design, they typically use dimensional modeling and a denormalized schema. 
- Optimized for analytics - OLAP
- Organized for reading/aggregating data
- Usually read-only
- Contains data from multiple sources
- Massively Parallel Processing (MPP)
- Typically uses a denormalized schema and dimensional modeling

### Data marts
- Subset of data warehouses
- Dedicated to a specific topic

### Data lakes
- Store all types of data at a lower cost: e.g., raw, operational databases, IoT device logs, real-time, relational and non-relational
- Retains all data and can take up petabytes
- Schema-on-read as opposed to schema-on-write
- Need to catalog data otherwise becomes a data swamp
- Run big data analytics using services such as Apache Spark and Hadoop
- Useful for deep learning and data discovery because activities require so much data

## Database Design

- Determines how data is logically stored. How is data going to be read and updated?
- Uses database models: high-level specifications for database structure. Most popular: relational model. Some other options: NoSQL models, object-oriented model, network model
- Uses schemas: blueprint of the database. Defines tables, fields, relationships, indexes, and views. When inserting data in relational databases, schemas must be respected

The first step to database design is data modeling. This is the abstract design phase, where we define a data model for the data to be stored. There are three levels to a data model: 
1. A conceptual data model describes what the database contains, such as its entities, relationships, and attributes. Tools: data structure diagrams, e.g., entity-relational diagrams and UML diagrams.
2. A logical data model decides how these entities and relationships map to tables. Tools: database models and schemas, e.g., relational model and star schema.
3. A physical data model looks at how data will be physically stored at the lowest level of abstraction. Tools: partitions, CPUs, indexes, backup systems and tablespaces.

These three levels of a data model ensure consistency and provide a plan for implementation and use.

### Dimensional Modelling

Dimensional modeling is an adaptation of the relational model specifically for data warehouses. It's optimized for OLAP type of queries that aim to analyze rather than update. To do this, it uses the star schema. Tends to be easy to interpret and extend.

**Elements**

Dimensional models are made up of two types of tables: fact and dimension tables. What the fact table holds is decided by the business use-case. It contains records of a key metric, and this metric changes often. Fact tables also hold foreign keys to dimension tables. Dimension tables hold descriptions of specific attributes and these do not change as often. 

Fact tables
- Decided by business use-case
- Holds records of a metric
- Changes regularly
- Connects to dimensions via foreign keys

Dimension tables
- Holds descriptions of attributes
- Does not change as often

### Star Schema

The star schema is the simplest form of the dimensional model. Some use the terms "star schema" and "dimensional model" interchangeably. The star schema is made up of two tables: fact and dimension tables. Fact tables hold records of metrics that are described further by dimension tables. The snowflake schema is an extension of the star schema. The star schema extends one dimension, while the snowflake schema extends over more than one dimension. This is because the dimension tables are normalized.

### Normalization

Normalization is a technique that divides tables into smaller tables and connects them via relationships. The goal is to reduce redundancy and increase data integrity. The basic idea is to identify repeating groups of data and create new tables for them. Normalization ensures better data integrity
1. Enforces data consistency - Must respect naming conventions because of referential integrity, e.g.,'California', not 'CA' or'california'
2. Safer updating, removing, and inserting. Less data redundancy = less records to alter
3. Easier to redesign by extending. Smaller tables are easier to extend than larger tables

### Normal forms (NF)
Ordered from least to most normalized:
- First normal form (1NF)
- Second normal form (2NF)
- Third normal form (3NF)
- Elementary key normal form (EKNF)
- Boyce-Codd normal form (BCNF)
- Fourth normal form (4NF)
- Essential tuple normal form (ETNF)
- Fifth normal form (5NF)
- Domain-key Normal Form (DKNF)
- Sixth normal form (6NF)


  </details>
