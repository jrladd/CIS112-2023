% SQL
% CIS 112, Dr. Ladd

# Structured Query Language

creates rules for defining and manipulating databases.

## SQL works in Postgres

but it's not the same *as* Postgres.

Postgres is the program, SQL is the language.

# Data Definition Language (DDL)

## Consider the groceries example...

date|item|price|quantity
--|---|--|--
8-11-2023|Apple|$3.99|2
8-14-2023|CokeZero|$4.99|3
8-14-2023|Apple|$3.99|5

## ... and its ERD.

![](img/grocery_erd2.png)

## You can CREATE individual schemas.

```sql
CREATE SCHEMA "groceries";
```

Always use quotes for attributes and names!

## You can SET that schema as your `search_path`.

You should do this at the beginning of every SQL script in NotePad++. (Save files with `.sql` extension.)

```sql
SET search_path TO "groceries";
```

## You can CREATE an entire TABLE with one command.

```sql
CREATE TABLE "cost" (
    "item" TEXT PRIMARY KEY,
    "price" FLOAT(3) NOT NULL
);
```

Make sure you include data types, constraints, and primary keys.

## The Postgres documentation is a huge help!

- [The SQL Language](https://www.postgresql.org/docs/14/tutorial.html)
- [Table Basics](https://www.postgresql.org/docs/14/ddl-basics.html)
- [Data Types](https://www.postgresql.org/docs/14/datatype.html)

## Sometimes your table will have a composite PRIMARY KEY.

```sql
CREATE TABLE "purchase" (
    "date" DATE NOT NULL,
    "item" TEXT NOT NULL,
    "quantity" INTEGER NOT NULL,
    PRIMARY KEY ("date", "item")
);
```

## Delete tables with DROP and CASCADE.

DROP TABLE removes the table and the CASCADE argument removes objects that depend on the table.

```sql
DROP TABLE "purchase" CASCADE;
```

## Add CONSTRAINTs to individual columns or the whole table.

Use the [Postgres Documentation](https://www.postgresql.org/docs/14/ddl-constraints.html) for a complete guide.

. . .

You've already done this with PRIMARY KEY and NOT NULL!

## You can make one or many columns UNIQUE.

```sql
CREATE TABLE "cost" (
    "item" TEXT UNIQUE,
    "price" FLOAT(3)
);
```

or

```sql
CREATE TABLE "cost" (
    "item" TEXT,
    "price" FLOAT(3),
    UNIQUE ("item")
);
```

## Add foreign keys with the REFERENCES constraint.

- Avoid errors with appropriate *flow*: i.e. you must reference a table that already exists!
- If you don't specify the columns, SQL will automatically use the reference table's *primary key*.

## Examples of the REFERENCES constraint

```sql
CREATE TABLE "purchase" ( --Option 1: Column Constraint
    "date" DATE,
    "item" TEXT REFERENCES "cost" ("item"),
    "quantity" INTEGER NOT NULL,
    PRIMARY KEY ("date", "item")
);
```

```sql
CREATE TABLE "purchase" ( --Option 2: Table Constraint
    "date" DATE,
    "item" TEXT,
    "quantity" INTEGER NOT NULL,
    PRIMARY KEY ("date", "item"),
    FOREIGN KEY ("item") REFERENCES "cost" ("item")
);
```

## Create custom constraints with CHECK.

```sql
CREATE TABLE "cost" (
    "item" TEXT PRIMARY KEY,
    "price" FLOAT(3) CHECK ("price" > 0)
);
```

## You try it!

1. Completely delete the groceries schema (but save your .sql files).
2. Rewrite your DDL script with all original constraints.
3. Add a constraint for the correct foreign key in the "purchase" table.
4. Add a constraint to prevent "price" from being negative.
5. Re-run your INSERT commands from last time to repopulate the database.
6. Look at the schemas and data (with SELECT) to see if it worked.

## Change tables with ALTER.

A few examples:

```sql
-- Add a column
ALTER TABLE "cost" ADD COLUMN "weight" FLOAT(4);

-- Drop a column
ALTER TABLE "purchase" DROP COLUMN "quantity";

-- Rename a column
ALTER TABLE "purchase" RENAME COLUMN "date" TO "day";

-- Add (or remove) a constraint
ALTER TABLE "purchase" ADD FOREIGN KEY ("item") REFERENCES "cost";
```

More in the [Postgres documentation](https://www.postgresql.org/docs/14/ddl-alter.html).

# Data Manipulation Language (DML)

## INSERT data into your tables.

```sql
INSERT INTO "cost" VALUES ('Apple', 3.99);
```

## Or INSERT into specific columns.

```sql
INSERT INTO "cost" (item) VALUES ('CokeZero');
```

This will only work if the other columns can be NULL!

## You try it!

Enter the command for inserting data into the purchase table.

HINT: You may need to look at the Postgres documentation for data types.

## Check if your data is there with SELECT.

```sql
SELECT * from "cost";
```

```sql
SELECT * from "purchase";
```

We'll learn a lot more about SELECT later on...

## DELETE records from your table.

```sql
DELETE FROM "purchase" WHERE "item"='Apple';
```

- Delete takes a *condition* with WHERE (**not IF**)
- Type of quotation marks is important! Tables and column names in double; values in single.
- `DELETE FROM "purchase";` will delete **ALL** the data without warning.

## You try it!

Delete CokeZero from the `cost` table.

## Copy an entire table from a CSV file.

```sql
\copy "Musical Instrument" FROM 'musical_instruments.csv' WITH CSV HEADER
```

- The column names **MUST** be in the same order.
- The CSV must be in the same directory as your psql login.

## Change records with UPDATE.

```sql
-- Change one or many rows
UPDATE "purchase" SET "quantity" = 6 WHERE "quantity" = 5;

-- Change all rows based on original value
UPDATE "cost" SET "price" = "price" * 100;

-- Change more than one column
UPDATE "cost" SET "item" = 'Banana', "price" = 599 WHERE "item" = 'Apple';
```

All this and more in the [Postgres documentation](https://www.postgresql.org/docs/14/dml-update.html).

## Get data while modifying rows with RETURNING.

```sql
INSERT INTO "cost" VALUES ('CokeZero', 4.99) RETURNING "item";

UPDATE "cost" SET "price" = "price" * 100 RETURNING "price";
```

As always, more in the [Postgres documentation](https://www.postgresql.org/docs/14/dml-returning.html).

# Queries (DML)

## ERDs are *essential* for queries.

You need them for column names, but also for capitalization, combining different tables, etc.

## SELECT is the command for **all** queries.

In its simplest form, you can use it to do simple arithmetic or to get all rows from a table.

```sql
SELECT 5 * 9;
```

```sql
SELECT * FROM "purchase";
```

```sql
SELECT "item", "quantity" FROM "purchase";
```

## Perform arithmetic on columns and give them new names with AS.

```sql
SELECT "item", "quantity", "quantity" + 5 AS "updated_quantity" FROM "purchase";
```

## If the column isn't numeric (i.e. money or a date), you may have to **type cast** to avoid errors:

```sql
SELECT "item", "quantity", "quantity"::NUMERIC + 5 AS "updated_quantity" FROM "purchase";
```

Other forms of type casting:

```
"quantity"::NUMERIC::INTEGER
CAST("quantity" AS NUMERIC)
CAST(CAST("quantity" AS NUMERIC) AS INTEGER))
```

## Sort results with ORDER BY.

```sql
SELECT "item", "quantity" FROM "purchase" ORDER BY "item";
```

ORDER BY can also be used with arithmetic expressions and newly created columns!

## Get specific rows with WHERE conditions.

```sql
SELECT "item", "date" FROM "purchase" WHERE "item" = 'Apple';
```

## Get a set number of rows with LIMIT.

```sql
SELECT * FROM "purchase" LIMIT 2;
```

Use OFFSET to start somewhere other than the beginning (which is row 0):

```sql
SELECT * FROM "purchase" LIMIT 2 OFFSET 1;
```

It's recommended to use LIMIT with ORDER BY always!

## Remember, you can always refer back to the documentation:

SELECT queries: <https://www.postgresql.org/docs/14/queries-overview.html>

ORDER BY and AS: <https://www.postgresql.org/docs/14/queries-order.html>

LIMIT and OFFSET: <https://www.postgresql.org/docs/14/queries-limit.html>

## You Try It!

1. Finish adding tables and data to the Music schema.
2. Write queries to answer the following questions:
- What pitches were used in files where a clarinet was played?
- What would each instrument cost if I got a 10% discount?
- Which tone colors are visible?
- What are the last three note pitches?
3. Save all of these queries in a separate .sql file, which comments that explain what each one does.
4. When you finish, return to the other practice data sets.

## Aggregate functions let you calculate based on columns.

```sql
SELECT COUNT("item") FROM "purchase";

SELECT AVG("price"::NUMERIC) FROM "cost";
```

Remember to add type casts as needed!

## Key aggregate functions:

- COUNT()
- AVG()
- SUM()
- MIN()
- MAX()

You can use the full list from the [Postgres documentation](https://www.postgresql.org/docs/14/functions-aggregate.html)!

## Use DISTINCT to work with unique values.

```sql
SELECT COUNT(DISTINCT "item") FROM "purchase";
```

## GROUP rows to summarize your table.

```sql
SELECT "item", AVG("quantity") FROM "purchase" GROUP BY "item";
```

Use HAVING to limit groups.

<https://www.postgresql.org/docs/14/queries-table-expressions.html#QUERIES-GROUP>

## Subqueries define connections between tables.

There are several expressions for subqueries in [the documentation](https://www.postgresql.org/docs/14/functions-subquery.html).

Use foreign keys to guide you! This is ideal for *optional* relations.

```sql
-- This should all be on one line in your .sql file
SELECT AVG("price"::NUMERIC) FROM "cost" WHERE "item" 
    IN (SELECT "item" FROM "purchase" WHERE "quantity" < 3);
```

## You try it!

Write queries to answer the following for the "music" schema:

1. What is the average cost of all instruments?
2. What is the average cost of *each* instrument (since some have multiple manufacturers)?
3. What is the maximum frequency of pitch colors with a WvMin (for *color*) of less than 500?
4. How many individual instrument families are there?
