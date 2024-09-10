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
CREATE TABLE cities (
  name VARCHAR(50),
  country VARCHAR(50),
  population INTEGER,
  area INTEGER
);
```

- The `CREATE TABLE` keyword ...
- `cities` is the identifier

### Inserting data into a table

```sql

```

- 

### Retrieving records
### Filtering records
### Relating records with joins
### Aggregation of records
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
