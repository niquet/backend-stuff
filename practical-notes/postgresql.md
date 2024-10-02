# PostgreSQL

- 

## (Relational) Database semantics

- Databases are used to store information (in tables)
- Either in memory or on hard drive
- Eventually the data is retrieved at some point in future time
- Database design process
  - What kind of _thing_ are we storing?
  - What properties does this thing have?
  - What _type_ of data does each of those properties contain?
- Object model
  - Queries:
  - Data types:
  - Rows: also referred to as record; each row represents a thing we are storing
  - Columns: each column records one property about a thing
  - Tables: collection of records; a table stores a list of records (e.g., a list of to-do items, a list of photos, or a list of cities)
  - Schemas:
  - Databases:

---

## PostgreSQL semantics

- To work with a PostgreSQL database, you have to connect to it with a client
- A client can be any piece of software such as an API server, a utility program, or one of many other types of clients
- SQL tells our database some information we want to put inside of it, that we want to retrieve, update or delete
- SQL is the primary way we interact with a PostgreSQL database
- Challenges of Postgres
  - Writing efficient queries to _retrieve_ information
  - Designing the schema, or structure, of the database
  - Understanding when to use advanced features
  - Managing the database in a production environment
- 

---

## SQL language semantics

- SQL is a communication language used to interface/interact with databases
- It's supported by many other databases and not just PostgreSQL
- _Keywords_ tell the database that we want to do something
- Convention is to always write keywords out in capital letters
- _Identitifiers_ tell the database what thing we want to act on
- Convention is to always write identifiers out in lower case letters
- 

### Column data types

- `VARCHAR(50)`: variable length character (text); inserting a string longer than 50 characters results in an error
- `INTEGER`: number without a decimal between -2,147,483,647 and +2,147,483,647; anything larger or smaller results in an error
- 

### Creating a table

```sql
-- CREATE A TABLE USING THE CREATE TABLE KEYWORD
CREATE TABLE table_name (
  first_column TEXT,
  second_column INTEGER
);

-- RETRIEVE DATA FROM SPECIFIED COLUMNS ONLY
SELECT specify, column, names FROM table_name; 
```

- Creates a table named `table_name` with two columns
- The first column is named first_column and has a data type of text; the second column has the name second_column and the type integer
- The table and column names follow the identifier syntax explained in Section 4.1.1.
- The type names are usually also identifiers, but there are some exceptions
- Note that the column list is comma-separated and surrounded by parentheses

### Inserting data into a table

```sql
-- INSERT ONE USING THE INSERT INTO KEYWORD
INSERT INTO table_name (list/value, of, columns, that, will, be, provided)
VALUES (<comma separated values to insert>);

-- INSERT MULTIPLE ROWS AT ONCE USING THE INSERT INTO KEYWORD
INSERT INTO table_name (list/value, of, columns, that, will, be, provided)
VALUES
	(<comma separated values to insert>),
	(<comma separated values to insert>),
	-- ...
	(<comma separated values to insert>);
```

- Order of columns in first set of parentheses does not have to match the order of columns in the given table
- However, order of provided values in second set of parentheses has to match the one in the first set

### Retrieving records

```sql
-- RETRIEVE DATA FROM ALL OF THE COLUMNS
SELECT * FROM table_name;

-- RETRIEVE DATA FROM SPECIFIED COLUMNS ONLY
SELECT specify, column, names FROM table_name; 
```

- `*` refers to retrieving data from all of the columns inside a given table

### SQL statement execution order

```sql
SELECT column      -- Third
	FROM table       -- First
	WHERE condition  -- Second
```

### Updating records

```sql
UPDATE table_name
SET column=value
WHERE condition;
```

- `SET` keyword describes the property we want to change on some record

### Deleting records

```sql
DELETE FROM table_name
WHERE condition;
```

- When deleting records that are referenced to by a foreign key, update the foreign key to `NULL` to dereference the record ➡ ==on delete options== to describe that should happen if a record is deleted that is still being referenced to
- `ON DELETE RESTRICT` – throw an error (==default behavior==)
- `ON DELETE NO ACTION` – throw an error
- `ON DELETE CASCADE` – delete the referencing record too
- `ON DELETE SET NULL` – set the foreign key of the referencing record to `NULL`
- `ON DELETE SET DEFAULT` – set the foreign key of the referencing record to a default value, if one is provided
- On delete options are set after the foreign key column definition within the create table statement of the column to which they're applied

### Filtering records

SELECT * FROM table_name
WHERE condition;
```

- Filters are applied using the `WHERE` keyword
- `WHERE` keyword allows to filter the set of results we get back from a query
- Comparison math operators that can be used inside a where statement include `=`, `<=`, `>`, `<`, `<>` or `!=`, `>=`, `BETWEEN`, `IN`, `NOT IN`
- *Compound checks* allow to apply multiple conditions at once to filter the set of results using the `AND` and `OR` keywords within a `WHERE` clause
- Calculations can be used within `WHERE` clauses as well

### Relating records with joins
### Aggregation of records

- In SQL, data can be transformed or processed before receiving it using operators
- Available operators include `+`, `-`, `*`, `/`, `^` (exponent), `|/` (sqrt), `@` (abs), `%`

- Available string operators include `||` (join)
- Available string functions include `CONCAT()` (join), `LOWER()` (lower case), `LENGTH()` (num chars), `UPPER()` (upper case)

### Working with large datasets
### Unions and intersections with sets
### Assembling queries with sub-queries
### Selecting distinct records
### Utility operators, keywords, and functions

---

## PostgreSQL mechanics

### Local PostgreSQL installation
### PostgreSQL complex data types
### Implementing database design patterns
### 

---

## Database-side validation and constraints

---

## Database structure design patterns

---

## Credits

- [SQL and PostgreSQL: The Complete Developer's Guide]() by [Stephen Grider](https://deliveryhero.udemy.com/user/sgslo/)
