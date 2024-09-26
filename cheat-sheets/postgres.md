# PostgreSQL Cheat Sheet

![style](/misc/hack.svg#foo)

## Locks

- There are five lock categories and over 12 individual lock types in Postgres
- You should know which commands conflict and can't run at the same time (blocking each other)
- 

### Table locks

- There are eight types of table locks in Postgres
- Transactions can have multiple locks on the same table
- Some of thos locks conflict, some don't
- You can view the table locks in mode column in `pg_locks` table
- 

### ACCESS EXCLUSIVE

- `ACCESS EXCLUSIVE` is the most aggressive table lock
- Nothing can be done to a table after a command obtains this lock type on it
- This lock type conflicts with all other table locks, including system operations such as `VACUUM`

```sql
--LIST OF POSTGRES COMMANDS THAT OBTAIN ACCESS EXCLUSIVE TABLE LOCK
DROP TABLE
TRUNCATE 
REINDEX
CLUSTER
-- E.G., VACUUM FULL BLOCKS YOU FROM SELECTING OR UPDATING A TABLE
VACUUM FULL
REFRESH MATERIALIZED VIEW
-- DIFFERENT ALTER TABLES OBTAIN DIFFERENT TYPE OF LOCKS
-- SOME ALTERS BLOCK CERTAIN OPERATIONS WHILE OTHERS MIGHT NOT
ALTER INDEX SET TABLESPACE
ALTER INDEX ATTACH PARTITION
ALTER INDEX SET FILLFACTOR
ALTER TABLE ADD COLUMN
ALTER TABLE DROP COLUMN
ALTER TABLE SET DATA TYPE
ALTER TABLE SET/DROP DEFAULT
ALTER TABLE DROP EXPRESSION
ALTER TABLE SET SEQUENCE
ALTER TABLE SET STORAGE
ALTER TABLE SET COMPRESSION
ALTER TABLE ALTER CONSTRAINT 
ALTER TABLE DROP CONSTRAINT
ALTER TABLE ENABLE/DISABLE RULE
ALTER TABLE ENABLE/DISABLE ROW LEVEL SECURITY
ALTER TABLE SET TABLESPACE
ALTER TABLE RESET STORAGE
ALTER TABLE INHERIT PARENT
ALTER TABLE RENAME
```

- Any of the commands that acquire `ACCESS EXCLUSIVE` perform sergical changes to the layout of the table
- This would otherwise break consistency when conflicting commands are run

### ACCESS SHARE

- `ACCESS SHARE` is the lightest table lock
- Indicates that someone is reading the table
- The lock is acquired whether a single row, all rows, or no rows are read
- Only `SELECT` and `COPY TO` acquire this lock

```sql
--LIST OF POSTGRES COMMANDS THAT OBTAIN ACCESS SHARE TABLE LOCK
SELECT
COPY TO
```

- This lock type only conflicts with the `ACCESS EXCLUSIVE` lock

### EXCLUSIVE

- `EXCLUSIVE` is similar to `ACCESS EXCLUSIVE`, but it doesn't conflict with reads acquired by `ACCESS SHARE`
- You can `SELECT` while an `EXCLUSIVE` table lock is on the table

```sql
--LIST OF POSTGRES COMMANDS THAT OBTAIN EXCLUSIVE lock
REFRESH MATERIALIZED VIEW CONCURRENTLY
```

- Refresh materialized view acquires an `ACCESS EXCLUSIVE` lock blocking selects
- Postgres added both a new lock type `EXCLUSIVE` which conflicts with everything except `ACCESS SHARE` and then made a new command to allow refreshing the view concurrently
- Result: if you refresh your materialized view concurrently your table can’t be edited but can be read

### ROW SHARE

- `ROW SHARE` is similar to `ACCESS SHARE` but was designed for the `SELECT FOR`s command family
- `SELECT FOR UPDATE`, `SELECT FOR SHARE` and others work on rows (table lock)
- These kind of commands actually acquire two types of locks: row locks and the `ROW SHARE` row locks

```sql
--LIST OF POSTGRES COMMANDS THAT OBTAIN ROW SHARE table lock
SELECT FOR UPDATE
SELECT FOR NO KEY SHARE
SELECT FOR SHARE
SELECT FOR KEY SHARE
```

- This lock type conflicts with `ACCESS EXCLUSIVE` and the `EXCLUSIVE` lock
- While you can do normal `SELECT`s while refreshing your materialized view concurrently, you can’t really do a `SELECT FOR SHARE`

### ROW EXCLUSIVE
### SHARE ROW EXCLUSIVE
### SHARE
### SHARE UPDATE EXCLUSIVE

## Row locks

### FOR UPDATE
### FOR NO KEY UPDATE
### FOR SHARE
### FOR KEY SHARE

## Page locks

## Dead locks

## Advisory locks

## Weak locks



## Credits

- [Postgres Locks — A Deep Dive](https://medium.com/@hnasr/postgres-locks-a-deep-dive-9fc158a5641c) by Hussein Nasser
- 
