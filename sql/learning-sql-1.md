# Learning SQL 1

A document to help with learning how to write simple SQL queries. One can use
the [Online Database Playground](https://pgplayground.com/) to play with the
provided SQL examples.

## SQL Queries - Lecture 1

The lecture is structured such that one can copy the SQL block into the
The click on the red `Run Queries` button and have a look at the query results.

Each section covers one of following aspect:

- create a table `CREATE TABLE ... ;`
- populate it with some values `INSERT INTO ... ;`
- select records `SELECT ... FROM ... ;`
- filter result set with `SELECT ... FROM ... WHERE ... ;`
- aggregate query `SELECT ... FROM ... GROUP BY ... ;`

### SQL Query Structure

The structure of a SQL query is:

```sql
-- get all columns from table table_name in schema schema_name
select * from schema_name.table_name;

-- same using the default schema name (which one can set)
select * from table_name;
```

A couple of key points worth pointing out:

- queries start with `SELECT`
- queries name the source (e.g. table) from where to get the data
- queries end with a semicolon `;`
- queries list the column names to get, or `*` to get all columns
- the schema name is optional as there is a default schema name
- lines starting with two dashes `--` are comments

Some more "esoteric" points:

- unless in double quotes `"` the table and column names **are not case sensitive**.

### Common Preparation

All of the following sections use this common setup that creates and populates
the table `employees`.

> Feel free to ignore all these statements. They will be covered later.

```sql
create table employees(id int, first_name text, last_name text, hire_date date, department_id int, primary key(id));

-- ***
-- populate the table
--
insert into employees values (100, 'Steven'   ,'Mayer'     ,'1984-06-15', 3);
insert into employees values (101, 'Maria'    ,'Wang'      ,'1981-09-21', 5);
insert into employees values (102, 'Lex'      ,'De Haan'   ,'1997-01-13', 5);
insert into employees values (103, 'Alexander', 'Bock'     ,'1992-07-08', 7);
insert into employees values (104, 'Bruce'    ,'Ernst'     ,'1991-05-17', 7);
```

### Basic Queries

```sql
create table employees(id int, first_name text, last_name text, hire_date date, department_id int, primary key(id));

-- ***
-- populate the table
--
insert into employees values (100, 'Steven'   ,'Mayer'     ,'1984-06-15', 3);
insert into employees values (101, 'Maria'    ,'Wang'      ,'1981-09-21', 5);
insert into employees values (102, 'Lex'      ,'De Haan'   ,'1997-01-13', 5);
insert into employees values (103, 'Alexander', 'Bock'     ,'1992-07-08', 7);
insert into employees values (104, 'Bruce'    ,'Ernst'     ,'1991-05-17', 7);

select * from employees;
select first_name, hire_date from employees;
select first_name, hire_date from employees order by hire_date;
select first_name, hire_date from employees order by hire_date asc;
select first_name, hire_date from employees order by hire_date desc;
```

1. select all records and all columns
2. select only first name and hire date
3. like Q2 but order by hire date (ascending)
4. like Q3 but with explicit order direction `asc`
5. like Q4 but with explicit descending order `desc`

### Add Filter Condition - WHERE

One can add a condition to a query by adding `WHERE condition` to the SQL query. Some additional rules to keep in mind:

- Multiple condition can be combined with `AND` and `OR`.
- It can be necessary to add brackets to tell which conditions go together.
- A `AND` condition means both sides must be true.
- A `OR` condition means that it suffices if either or both sides are true.

Here is a set of sample queries to get started:

```sql
create table employees(id int, first_name text, last_name text, hire_date date, department_id int, primary key(id));

-- ***
-- populate the table
--
insert into employees values (100, 'Steven'   ,'Mayer'     ,'1984-06-15', 3);
insert into employees values (101, 'Maria'    ,'Wang'      ,'1981-09-21', 5);
insert into employees values (102, 'Lex'      ,'De Haan'   ,'1997-01-13', 5);
insert into employees values (103, 'Alexander', 'Bock'     ,'1992-07-08', 7);
insert into employees values (104, 'Bruce'    ,'Ernst'     ,'1991-05-17', 7);

select * from employees where hire_date < '1990/01/01';
select * from employees where hire_date < '1990/01/01' AND department_id = 5;
select * from employees where hire_date < '1990/01/01' AND department_id = 5 ORDER BY first_name;
```

The three queries do:

1. select all employees that were hired before 1990
2. select all employees that were hired before 1990 and work in department #5
3. same as 2 but order the result by the first name

### Aggregate Query - GROUP BY

One can aggregate multiple records into a summary by adding a `GROUP BY` clause.

- The `group by` lists all columns by which to group, i.e. every existing distinct combination is a row in the result
- The other columns must be aggregated by an aggregate function such as `count()`, `min()`, `max()`, and so forth.

```sql
create table employees(id int, first_name text, last_name text, hire_date date, department_id int, primary key(id));

-- ***
-- populate the table
--
insert into employees values (100, 'Steven'   ,'Mayer'     ,'1984-06-15', 3);
insert into employees values (101, 'Maria'    ,'Wang'      ,'1981-09-21', 5);
insert into employees values (102, 'Lex'      ,'De Haan'   ,'1997-01-13', 5);
insert into employees values (103, 'Alexander', 'Bock'     ,'1992-07-08', 7);
insert into employees values (104, 'Bruce'    ,'Ernst'     ,'1991-05-17', 7);

select department_id, count(*) from employees group by department_id;
select department_id, count(*) from employees group by department_id order by department_id;
select department_id, count(*) from employees where hire_date < '1990/01/01' group by department_id;
select department_id, count(*), min(hire_date) first_hire, max(hire_date) last_hire from employees group by department_id;
```

The four queries do compute:

1. count the total number of employees working in each department
2. same as Q1 but order the result set by the department ID
3. count only hires before 1990
4. add first and last hire date for each department

## Extra Chapter - Tables

### Tables - The Fundamentals

As for tables, one can think of

- tables like spreadsheets
- schema are like folder in which spreadsheets are organized
- if no schema name is specified the default schema is used
- tables have a structure, that is column names and column types
- tables typically have a primary key than uniquely identifies a record
- a row in a table is called a record

### Create Table SQL Statement

One creates table with a `CREATE TABLE` statement:

```sql
create table employees(id int, first_name text, last_name text, birthday date, primary key(id));
```

The syntax is `create table(col1 type, col2 type, ..., primary key(colx));`:

- list all the column names and their type as a comma `,` separated list
- the list is in brackets after `create table`
- the statement ends with a semicolon `;`
- the last part is primary key that list all columns that together uniquely identify a record
- it is common and a best practice to have a primary key but it is not required

