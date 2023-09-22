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

## COPY an entire table from a CSV file.

```sql
COPY sample_table_name FROM 'C:\sampledb\sample_data.csv' DELIMITER ',' CSV HEADER;
```

- The column names **MUST** be in the same order.

