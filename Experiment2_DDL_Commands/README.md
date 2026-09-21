# Experiment 2: DDL Commands

## AIM
To study and implement DDL commands and different types of constraints.

## THEORY

### 1. CREATE
Used to create a new relation (table).

**Syntax:**
```sql
CREATE TABLE (
  field_1 data_type(size),
  field_2 data_type(size),
  ...
);
```
### 2. ALTER
Used to add, modify, drop, or rename fields in an existing relation.
(a) ADD
```sql
ALTER TABLE std ADD (Address CHAR(10));
```
(b) MODIFY
```sql
ALTER TABLE relation_name MODIFY (field_1 new_data_type(size));
```
(c) DROP
```sql
ALTER TABLE relation_name DROP COLUMN field_name;
```
(d) RENAME
```sql
ALTER TABLE relation_name RENAME COLUMN old_field_name TO new_field_name;
```
### 3. DROP TABLE
Used to permanently delete the structure and data of a table.
```sql
DROP TABLE relation_name;
```
### 4. RENAME
Used to rename an existing database object.
```sql
RENAME TABLE old_relation_name TO new_relation_name;
```
### CONSTRAINTS
Constraints are used to specify rules for the data in a table. If there is any violation between the constraint and the data action, the action is aborted by the constraint. It can be specified when the table is created (using CREATE TABLE) or after it is created (using ALTER TABLE).
### 1. NOT NULL
When a column is defined as NOT NULL, it becomes mandatory to enter a value in that column.
Syntax:
```sql
CREATE TABLE Table_Name (
  column_name data_type(size) NOT NULL
);
```
### 2. UNIQUE
Ensures that values in a column are unique.
Syntax:
```sql
CREATE TABLE Table_Name (
  column_name data_type(size) UNIQUE
);
```
### 3. CHECK
Specifies a condition that each row must satisfy.
Syntax:
```sql
CREATE TABLE Table_Name (
  column_name data_type(size) CHECK (logical_expression)
);
```
### 4. PRIMARY KEY
Used to uniquely identify each record in a table.
Properties:
Must contain unique values.
Cannot be null.
Should contain minimal fields.
Syntax:
```sql
CREATE TABLE Table_Name (
  column_name data_type(size) PRIMARY KEY
);
```
### 5. FOREIGN KEY
Used to reference the primary key of another table.
Syntax:
```sql
CREATE TABLE Table_Name (
  column_name data_type(size),
  FOREIGN KEY (column_name) REFERENCES other_table(column)
);
```
### 6. DEFAULT
Used to insert a default value into a column if no value is specified.

Syntax:
```sql
CREATE TABLE Table_Name (
  col_name1 data_type,
  col_name2 data_type,
  col_name3 data_type DEFAULT 'default_value'
);
```

**Question 1**
--
-- Insert a book with ISBN 978-1234567890, Title Data Science Essentials, Author Jane Doe, Publisher TechBooks, and Year 2024 into the Books table.

```sql
INSERT INTO Books VALUES ('978-1234567890', 'Data Science Essentials', 'Jane Doe', 'TechBooks', 2024);
```

**Output:**

<img width="1233" height="201" alt="image" src="https://github.com/user-attachments/assets/84d23e1a-cd5c-48f8-9db6-dd5374d7ad73" />

**Question 2**
---
-- Create a table named Products with the following constraints:

ProductID should be the primary key.
ProductName should be NOT NULL.
Price is of real datatype and should be greater than 0.
Stock is of integer datatype and should be greater than or equal to 0.

```sql
CREATE TABLE Products ( ProductID INTEGER PRIMARY KEY, ProductName TEXT NOT NULL, Price REAL CHECK (Price > 0), Stock INTEGER CHECK (Stock >= 0));
```

**Output:**

<img width="1077" height="223" alt="image" src="https://github.com/user-attachments/assets/6a21eccc-9cdc-4542-849c-6479e4275d22" />

**Question 3**
---
-- Insert all books from Out_of_print_books into Books

Table attributes are ISBN, Title, Author, Publisher, YearPublished

```sql
INSERT INTO Books (ISBN, Title, Author, Publisher, YearPublished) SELECT ISBN, Title, Author, Publisher, YearPublished FROM Out_of_print_books;
```

**Output:**

<img width="1330" height="235" alt="image" src="https://github.com/user-attachments/assets/7eb48b03-82aa-4b0f-af73-41cbf7f184ad" />

**Question 4**
---
-- In the Cusomers table, insert a record where some fields are NULL, another record where all fields are filled without any NULL values, and a third record where some fields are filled, and others are left as NULL.

CustomerID  Name          Address      City        ZipCode
----------  ------------  ----------   ----------  ----------
306         Diana Prince  Themyscira
307         Bruce Wayne   Wayne Manor  Gotham      10007
308         Peter Parker  Queens                   11375

```sql
INSERT INTO Customers VALUES (306, 'Diana Prince', 'Themyscira', NULL, NULL), (307, 'Bruce Wayne', 'Wayne Mano', 'Gotham', 10007), (308, 'Peter Parker', 'Queens', NULL, 11375);
```

**Output:**

<img width="1290" height="292" alt="image" src="https://github.com/user-attachments/assets/6d21e369-89af-44a9-b35c-22e4eacb1e4b" />

**Question 5**
---
-- Create a table named Orders with the following constraints:
OrderID as INTEGER should be the primary key.
OrderDate as DATE should be not NULL.
CustomerID as INTEGER should be a foreign key referencing Customers(CustomerID).

```sql
CREATE TABLE Orders (OrderID INT PRIMARY KEY, OrderDate DATE NOT NULL, CustomerID INT, FOREIGN KEY (CustomerID) REFERENCES Customers(CustomerID));
```

**Output:**

<img width="1325" height="237" alt="image" src="https://github.com/user-attachments/assets/aaf6ba21-d9fc-4032-a758-418f2da1b80a" />

**Question 6**
---
-- Create a table named Employees with the following constraints:

EmployeeID should be the primary key.
FirstName and LastName should be NOT NULL.
Email should be unique.
Salary should be greater than 0.
DepartmentID should be a foreign key referencing the Departments table.

```sql
CREATE TABLE Employees ( EmployeeID INT PRIMARY KEY, FirstName TEXT NOT NULL, LastName TEXT NOT NULL, Email TEXT UNIQUE, Salary REAL CHECK (Salary > 0), DepartmentID INT, FOREIGN KEY (DepartmentID) REFERENCES Departments(DepartmentID));
```

**Output:**

<img width="1335" height="330" alt="image" src="https://github.com/user-attachments/assets/4b45ebda-41c1-429e-9127-70e4c4123245" />

**Question 7**
---
-- Write a SQL query to add birth_date attribute as timestamp (datatype) in the table customer 

Sample table: customer

 customer_id |   cust_name    |    city    | grade | salesman_id 
-------------+----------------+------------+-------+-------------
        3002 | Nick Rimando   | New York   |   100 |        5001
        3007 | Brad Davis     | New York   |   200 |        5001
        3005 | Graham Zusi    | California |   200 |        5002
 

```sql
ALTER TABLE customer ADD birth_date timestamp;
```

**Output:**

<img width="1372" height="215" alt="image" src="https://github.com/user-attachments/assets/21a34c3c-a6cb-488b-a462-964642f35b67" />

**Question 8**
---
-- Create a table named Tasks with the following columns:

TaskID as INTEGER
TaskName as TEXT
DueDate as DATE

```sql
CREATE TABLE Tasks ( TaskID INTEGER, TaskName TEXT, DueDate DATE );
```

**Output:**

<img width="1317" height="297" alt="image" src="https://github.com/user-attachments/assets/35ae4868-7c13-4d5d-9fbc-10d99577d734" />

**Question 9**
---
-- Write an SQL query to add two new columns, department_id and manager_id, to the table employee with datatype of INTEGER. The manager_id column should have a default value of NULL.

```sql
ALTER TABLE employee ADD department_id INTEGER;
ALTER TABLE employee ADD manager_id INTEGER DEFAULT NULL;
```

**Output:**

<img width="1187" height="265" alt="image" src="https://github.com/user-attachments/assets/7ff191a7-ef2e-4484-8658-75315103b584" />

**Question 10**
---
-- Create a table named Department with the following constraints:
DepartmentID as INTEGER should be the primary key.
DepartmentName as TEXT should be unique and not NULL.
Location as TEXT.

```sql
CREATE TABLE Department ( DepartmentID INTEGER PRIMARY KEY, DepartmentName TEXT NOT NULL UNIQUE, Location TEXT );
```

**Output:**

<img width="1373" height="185" alt="image" src="https://github.com/user-attachments/assets/c4640a4e-f2ff-40f2-aa41-6517d84d0a8c" />


## RESULT
Thus, the SQL queries to implement different types of constraints and DDL commands have been executed successfully.

<img width="1482" height="726" alt="image" src="https://github.com/user-attachments/assets/0665e6fe-f54f-451b-b19a-f32f1353fd91" />


