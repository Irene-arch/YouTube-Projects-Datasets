# YouTube-Projects-Datasets
This repo contains all the datasets for my YouTube videos. Updated as more videos are uploaded.

1. **Excel Call Center Dashboard** - [YouTube](https://youtu.be/VJVuDbIRWAc) | [Dataset Link](https://github.com/Irene-arch/YouTube-Projects-Datasets/blob/main/Human%20Resources.csv)
2. **SQL & PowerBI Project** - [YouTube](https://youtu.be/PzyZI9uLXvY) | [Dataset Link](https://github.com/Irene-arch/YouTube-Projects-Datasets/blob/main/Human%20Resources.csv)
3. **Data Cleaning in Python (FIFA21)** - [YouTube](https://youtu.be/7mYbrpfAU6k) | [Dataset Link](https://github.com/Irene-arch/YouTube-Projects-Datasets/blob/main/fifa21%20raw%20data%20v2.csv)
4. **Customer Behaviour Analysis Using SQL** - [YouTube](https://youtu.be/lHR1_j8DYFA) | [Dataset Link](https://github.com/Irene-arch/YouTube-Projects-Datasets/blob/main/Dannys%20Diner%20Template.sql)


<details close>
  <summary> <b> SQL CheatSheet - Beginner to Advanced </b> </summary>
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

### 1NF Rules

The requirement of 1st Normal Form is that table values must be atomic meaning the value cannot be divided into smaller units.
- Each record must be unique - no duplicate rows.
- Each cell must hold one value.

### 2NF Rules

A database is in 2nd Normal Form when it satisfies 1st Normal Form and non-key columns only depend on the table's PRIMARY KEY.
- Must satisfy 1NF AND If primary key is one column then automatically satisfies 2NF
- If there is a composite primary key then each non-key column must be dependent on all the keys.

### 3NF Rules

To satisfy 3rd Normal Form, we must first satisfy the requirements of 2nd Normal Form. 3rd Normal Form has an additional requirement that no transitive dependencies are present in the table. This means that non-key columns are solely dependent on the table PRIMARY KEY.
- Satisfies 2NF
- Doesn't allow transitive dependencies. This means that non-primary key columns can't depend on other non-primary key columns.

Transitive dependencies are relationships within a database table that involve three columns. Imagine a table exists which has multiple columns: X, Y, and Z. Column Y is determined by column X. So, if we know the value of column X, column Y is also known. In this scenario, the same relationship structure exists between column Y and column Z. Knowing the value of column Y removes all ambiguity about the value of column Z. The transitive dependency, in this case, is between column X and column Z. Knowing the value of column X leaves no ambiguity about the value of column Z due to both columns' relationship to column Y. This is the case, even though, column X and column Z are not directly related.

A database that isn't normalized enough is prone to three types of anomaly errors: update, insertion, and deletion. An update anomaly is a data inconsistency caused by data redundancy when updating. An insertion anomaly is when you're unable to add a new record due to missing attributes. The dependency between columns in the same table unintentionally restricts what can be inserted into the table. Deletion anomaly happens when you delete a record and unintentionally delete other data.

## Database Views

Virtual table that is not part of the physical schema
- Query, not data, is stored in memory
- Data is aggregated from data in tables
- Can be queried like a regular database table
- No need to retype common queries or alter schemas

SYNTAX

```sql
CREATE VIEW view_name AS
SELECT col1, col2
FROM table_name
WHERE condition;
```

You can query it as you would a normal table. It's important to keep track of the views in your database. To get all the views in your database, you can run a query on the INFORMATION_SCHEMA.views table. Note that this command is specific to PostgreSQL.

```sql
--- include system views
SELECT * FROM INFORMATION_SCHEMA.views;

--- exclude system views
SELECT * FROM information_schema.views
WHERE table_schema NOT IN ('pg_catalog','information_schema');
```

### Benefits of views
- Doesn't take up storage
- A form of access control. Hide sensitive columns and restrict what user can see
- Masks complexity of queries. Useful for highly normalized schema

### Granting and revoking access to a view

```sql
GRANT privilege(s) or REVOKE privilege(s)
ON object
TO role or FROM role
```

- Privileges: SELECT , INSERT , UPDATE , DELETE , etc
- Objects: table, view, schema, etc
- Roles: a database user or a group of database users

The update privilege on an object called ratings is being granted to public. PUBLIC is a SQL term that encompasses all users. All users can now use the UPDATE command on the ratings object.

```sql
GRANT UPDATE ON ratings TO PUBLIC;
```

The user db_user will no longer be able to INSERT on the object films.

```sql
REVOKE INSERT ON films FROM db_user;
````

### Updating a view

Not all views are updatable. However a view can be updated if:

- View is made up of one table
- Doesn't use a window or aggregate function

`UPDATE films SET kind = 'Dramatic' WHERE kind = 'Drama';`

Generally, avoid modifying data through views. It's usually a good idea to use views for read-only purposes only.

Dropping a view is straightforward with the DROP command. There are two useful parameters to know about: CASCADE and RESTRICT. Sometimes there are SQL objects that depend on views. For example, it's not unusual for views to build off of other views in larger databases. The RESTRICT parameter is the default and returns an error when you try to drop a view that other objects depend on. The CASCADE parameter will drop the view and any object that depends on that view.

`DROP VIEW view_name [ CASCADE | RESTRICT ];`

### Redefining a view

Say you want to change the query a view is defined by. To do this, you can use the CREATE OR REPLACE command. If a view_name exists, it is replaced by the new_query specified. However, there are limitations to this. The new query must generate the same column names, column order, and column data types as the existing query. The column output may be different, as long as those conditions are met. New columns may be added at the end. If this criteria can't be met, the solution is to drop the existing view and create a new one.

`CREATE OR REPLACE VIEW view_name AS new_query`

### [Altering a view](https://www.postgresql.org/docs/9.2/sql-alterview.html)

The auxiliary properties of a view can be altered. This includes changing the name, owner, and schema of a view.

### Materialised Views

There are 2 types of views: non materialised(normal virtual views) and materialised views(physically materialised).
- Stores the query results not the query. These query results are stored on disk. This means the query becomes precomputed via the view. When you query a materialized view, it accesses the stored query results on the disk, rather than running the query like a non-materialized view and creating a virtual table. Materialized views are refreshed or rematerialized when prompted/scheduled. By refreshed or rematerialized, I mean that the query is run and the stored query results are updated.
- Materialized views are great if you have queries with long execution time. The caveat is the data is only as up-to-date as the last time the view was refreshed. So, you shouldn't use materialized views on data that is being updated often, because then analyses will be run too often on out-of-date data. Materialized views are particularly useful in data warehouses.
- Data warehouses are typically used for OLAP, meaning more for analysis than writing to data.

**When to use materialized views**

- Long running queries
- Underlying query results don't change often
- Data warehouses because OLAP is not write-intensive. Save on computational cost of frequent queries

```sql
CREATE MATERIALIZED VIEW my_mv AS SELECT * FROM existing_table;

REFRESH MATERIALIZED VIEW my_mv;
```

Unlike non-materialized views, you need to manage when you refresh materialized views when you have dependencies.

## DATABASE ROLES & ACCESS CONTROL

Roles are used to manage database access permissions. A role is an entity that can function as a user and/or a group

- Empty role - `CREATE ROLE data_analyst;`

- Roles with some attributes set - `CREATE ROLE intern WITH PASSWORD 'PasswordForIntern' VALID UNTIL '2020-01-01';`

`CREATE ROLE admin CREATEDB;`

`ALTER ROLE admin CREATEROLE;`

The available privileges in PostgreSQL are: SELECT , INSERT , UPDATE , DELETE , TRUNCATE , REFERENCES , TRIGGER , CREATE , CONNECT ,TEMPORARY , EXECUTE , and USAGE

Add a user role to a group role - `GRANT data_analyst TO alex;`

**Benefits**
- Roles live on after users are deleted
- Roles can be created before user accounts
- Save DBAs time

**Pitfalls**
- Sometimes a role gives a specific user too much access. You need to pay attention

### Database Partitioning

When tables grow — we're talking hundreds of gigabytes or even terabytes here — queries tend to become slow. Even when we've set indices correctly, these indices can become so large they don't fit into memory. At a certain point, it can make sense to split a table up into multiple smaller parts. We call the process of doing this 'partitioning'.

**Table Partitioning**

There are two different types of partitioning.
- Vertical Partitioning - Split table even when fully normalized. Splits up a table vertically by its columns
- Horizontal Partitioning - Split the table over the rows.

```sql
CREATE TABLE sales (
  ...
  timestamp DATE NOT NULL
)
PARTITION BY RANGE (timestamp);

CREATE TABLE sales_2019_q1 PARTITION OF sales
  FOR VALUES FROM ('2019-01-01') TO ('2019-03-31');
...
CREATE TABLE sales_2019_q4 PARTITION OF sales
  FOR VALUES FROM ('2019-09-01') TO ('2019-12-31');
CREATE INDEX ON sales ('timestamp');
```

**Pros**
- Indices of heavily-used partitions fit in memory
- Move to specific medium: slower vs. faster
- Used for both OLAP as OLTP

**Cons**
- Partitioning existing table can be a hassle
- Some constraints can not be set eg the primary key.

**SQL DBMS**

- Relational DataBase Management System(RDBMS)
- Based on the relational model of data
- Query language: SQL
- Best option when:Data is structured and unchanging, Data must be consistent

**NoSQL DBMS**
- Less structured
- Document-centered rather than table-centered
- Data doesn’t have to fit into well-defined rows and columns
- Best option when: Rapid growth, No clear schema definitions, Large quantities of data
- Types: key-value store, document store, columnar database, graph database

A key-value database stores combinations of keys and values. The key serves as a unique identifier to retrieve an associated value. Values can be anything from simple objects, like integers or strings, to more complex objects, like JSON structures. They are most frequently used for managing session information in web applications. For example, managing shopping carts for online buyers. An example DBMS is Redis.

Document stores are similar to key-value in that they consist of keys, each corresponding to a value. The difference is that the stored values, referred to as documents, provide some structure and encoding of the managed data. That structure can be used to do more advanced queries on the data instead of just value retrieval. A document database is a great choice for content management applications such as blogs and video platforms. Each entity that the application tracks can be stored as a single document. An example of a document store DBMS is mongoDB.

## CREATING POSTGERSQL DATABASES

By default database names cannot be longer than 31 characters and must start with a letter or underscore. Examples of valid names.

```sql
CREATE DATABASE db_name;
CREATE DATABASE my_db;
CREATE DATABASE_my_db;
```

### Schemas

A schema is similar to a directory on an operating system, however, instead of containing files, the schema contains a collection of tables. In fact, schemas can also contain other database objects including data types and functions.

Schemas have a number of use-cases. A primary use-case is providing a way for a database with multiple users to grant each user her own set of tables to use and manipulate without interfering with the data of other users.

Another important use of schemas is to provide a way to organize components of a database. Perhaps a company has a number of very distinct business units. Schemas provide a way for the company's data to be housed in a single database while having the components of the business that are represented in the database separated from each other through the use of a number of schemas -- one for each business unit.

By default, newly created tables are added to the public schema.

```sql
CREATE SCHEMA division1;

CREATE TABLE division1.school (
id serial PRIMARY KEY,
name TEXT NOT NULL,
mascot_name TEXT,
num_scholarships INTEGER DEFAULT 0
);
```

**Schema naming restrictions**

- Length of name less than 32
- Name begins with letter or underscore ("_")
- Schema name cannot begin with "pg_" as PostgreSQL reserves names with this prefix for system-level schemas.

**PostgreSQL Data Types**

The CHAR data type is used to represent a sequence of characters. It differs from VARCHAR in that values stored in a CHAR column do not vary in length. If a sequence is stored that is less than the fixed length, N, spaces are added to the end to ensure the string is of the expected length. Specifying a CHAR column without N defaults to a column that can only store a single character. This is the equivalent of `CHAR(1)`

Precision is the total number of digits in the number before and after the decimal point. Scale is the number of digits to the right of the decimal point. DECIMAL(precision, scale) ie DECIMAL(8,2) eg 100,000.56 - the comma is not included.

Boolean values can be used to represent the state of an object having one of three possible values. One possible value is a true state. The other possible value is a false state. A third possibility for a boolean column's value is a null entry that is used if the value is unknown.

Temporal values are used when representing a date and/or a time related to a table record. The most complete temporal data type is the TIMESTAMP type which stores both a date and time as the column value. Other types include DATE, TIME

### ACCESS CONTROL IN POSTGRES

Create a new user account
- Can create tables in the database.
- Have no access to tables created by other users.
```sql
CREATE USER newuseR;

-- create user with password
CREATE USER newuser WITH PASSWORD 'secret';

-- change user's password
ALTER USER newuser WITH PASSWORD 'new_password';
```

Users a a type of role. Group roles can also be defined. Superuser grants privileges.
Use the syntax below for update, select etc

```sql
GRANT INSERT ON table TO fin;
```

Some prvileges cannot be granted, such as modifying the table. It requires ownership

```sql
ALTER TABLE tablename OWNER TO fin;

-- add user to a group
ALTER GROUP name ADD USER lopez;

-- remove user from a group
REVOKE group FROM user;
```

### Temporary Tables

- They are appealing because they provide transient storage. 
- They are available for the duration of the database session, meaning they only temporarily tie database resources.
- Within your database session, they are available in multiple distinct queries.
- Temp tables are user specific, meaning they are available only to you as the creator.
- Creating a temporary copy of a slow table is a good way to make it faster to query.
```sql
CREATE TEMP TABLE name AS

CREATE TEMP TABLE usa_holidays AS
SELECT holiday, holiday_type
FROM world_holidays
WHERE country_code = 'USA';

ANALYZE usa_holidays;

SELECT * FROM usa_holidays
```
Finally, it is good practice to add the ANALYZE function after the table creation. ANALYZE returns no visible output but helps with the query execution plan.

ANALYZE calculates the records returned at different query points. It stores this information in the pg_statistics catalog. The query planner uses pg_statistics to estimate the runtime of the possible execution plans. The table statistics improve the query planner's ability to choose the optimal execution plan.

In SQL, as in algebra, there is a lexical order, the order as written. This differs from the logical order, the order as executed.

#### SQL Logical Order of Operations

FROM, WHERE, GROUP BY, SUM()/COUNT(), SELECT

|CLAUSE|PURPOSE|
|------|-------|
|FROM  |Provides direction to the table(s) if the query includes joins|
|WHERE|Filters or limits the records|
|GROUP BY|Places records into categories|
|SUM(),COUNT()|Aggregates|
|SELECT|Identifies columns to return|

  </details>


<details close>
  <summary> <b> Documenting Data Analysis Projects on GitHub </b> </summary>
  <br>
  
GitHub is a code hosting platform for version control and collaboration. It lets you and others work together on projects from anywhere.
  - Create a repo for the project and enable ReadMe.
  - Edit the ReadMe file and add various sections.
  - Update the sections with details of the projects including screenshots where appropriate.
  - Highlight the skills gained or demonstrated during the execution of the project.
  - Draw insight and conclusion.
  - Upload PowerBI or excel file into the rep.

## Data Cleaning in Python

To save the changes, you can do inplace=True or reassign to the same dataframe eg df = df.drop_duplicates() or to save changes to a column eg df["col_name"]= df["col_name"].somefunction
- Remove Duplicates - `df.drop_duplicates()`
- Remove columns not needed - `df.drop(columns="unwanted_column")`
- Remove unwanted characters from the string of a certain column `df["column_name"] = df["column_name"].str.strip("12._")` or left or right side of a string in a column - `df["column_name"] = df["column_name"].str.lstrip("...")` or rstrip.
- Remove special characters from a numeric column `df["Phone Number"].str.replace('[^a-zA-0-9]','')` - replace all the characters except for those in parentheses.
  If you need to convert the values in a column to a string, use a lambada function eg `df["Phone Number"].apply(lambda x: str(x))` and then now convert the values to the format you'd like.

`df["Phone Number"].apply(lambda x: x[0:3] + '-' + x[3:6] + '-' + x[6:10])`

- Split a column

  `df[["col_1", "col_2", "col_3"]] = df["Address"].str.split(",",2,expand=True)`
- Replace values represented as NaN with nothing/blanks - `df=df.fillna('')`
- The `df.index` is actually the numbering that is usually at the left of the dataframe after its been displayed.
  ```python
  for x in df.index:
    if df.loc[x, "col_name"] == 'Y':
      df.drop(x, inplace=True)
  ```

  Or you could do `df=df.dropna(subset="col_name"),inplace=True)`
- To reset the index - `df.reset_index(drop=True)`
</details>
