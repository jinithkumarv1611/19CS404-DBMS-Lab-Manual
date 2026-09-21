# Experiment 3: DML Commands

## AIM
To study and implement DML (Data Manipulation Language) commands.

## THEORY

### 1. INSERT INTO
Used to add records into a relation.
These are three type of INSERT INTO queries which are as
A)Inserting a single record
**Syntax (Single Row):**
```sql
INSERT INTO table_name (field_1, field_2, ...) VALUES (value_1, value_2, ...);
```
**Syntax (Multiple Rows):**
```sql
INSERT INTO table_name (field_1, field_2, ...) VALUES
(value_1, value_2, ...),
(value_3, value_4, ...);
```
**Syntax (Insert from another table):**
```sql
INSERT INTO table_name SELECT * FROM other_table WHERE condition;
```
### 2. UPDATE
Used to modify records in a relation.
Syntax:
```sql
UPDATE table_name SET column1 = value1, column2 = value2 WHERE condition;
```
### 3. DELETE
Used to delete records from a relation.
**Syntax (All rows):**
```sql
DELETE FROM table_name;
```
**Syntax (Specific condition):**
```sql
DELETE FROM table_name WHERE condition;
```
### 4. SELECT
Used to retrieve records from a table.
**Syntax:**
```sql
SELECT column1, column2 FROM table_name WHERE condition;
```
**Question 1**
--
Decrease the reorder level by 30 percent where the product name contains 'cream' and quantity in stock is higher than reorder level in the products table.

PRODUCTS TABLE

name               type
-----------------  ---------------
product_id         INT
product_name       VARCHAR(100)
category           VARCHAR(50)
cost_price         DECIMAL(10,2)
sell_price         DECIMAL(10,2)
reorder_lvl        INT
quantity           INT
supplier_id        INT
```sql
UPDATE products
SET reorder_lvl = reorder_lvl * 0.70
WHERE product_name LIKE '%cream%'
  AND quantity > reorder_lvl;
```

**Output:**

<img width="1357" height="295" alt="image" src="https://github.com/user-attachments/assets/1f385724-1337-4081-87c0-41e214da72c7" />

**Question 2**
---
Write a SQL statement to Update the grade of all customers in Chennai city as  5. 

Customer table (customer_id,cust_name,city,grade,salesman_id)
```sql
UPDATE Customer
SET grade = 5
WHERE city = 'Chennai';
```

**Output:**

<img width="1220" height="365" alt="image" src="https://github.com/user-attachments/assets/ab42a8e4-7101-4c6d-8fb5-36b7c17aa2a4" />

**Question 3**
---
Write a SQL statement to Increase the selling price per unit by 5% for product ID 15 who's sale is on '2023-01-31'.

sales(sale_id,sale_date,product_id,quantity,sell_price,total_sell_price)
```sql
UPDATE sales
SET sell_price = sell_price * 1.05
WHERE product_id = 15
  AND sale_date = '2023-01-31';
```

**Output:**

<img width="1335" height="358" alt="image" src="https://github.com/user-attachments/assets/2887cd64-c7f7-416e-b739-3ee3a0f836e1" />

**Question 4**
---
Update the 'Selling_Price' to add 10% extra margin for all products supplied by the supplier with id 6.

PRODUCTS TABLE

name               type
-----------------  ---------------
product_id         INT
product_name       VARCHAR(100)
category           VARCHAR(50)
cost_price         DECIMAL(10,2)
sell_price         DECIMAL(10,2)
reorder_lvl        INT
quantity           INT
supplier_id        INT
```sql
UPDATE Products
SET sell_price = ROUND(sell_price + (sell_price * 10 / 100), 1)
WHERE supplier_id = 6;
```

**Output:**

<img width="1298" height="317" alt="image" src="https://github.com/user-attachments/assets/2f3db16e-a25b-4985-9420-cafaf583afa9" />

**Question 5**
---
Write a SQL statement to Change the supplier name to 'A1 Suppliers' where the supplier ID is 8 in the suppliers table.

Table info

suppliers(supplier_id,supplier_name,contact_person,phone_number,email,address)
```sql
UPDATE suppliers
SET supplier_name = 'A1 Suppliers'
WHERE supplier_id = 8;
```

**Output:**

<img width="1557" height="332" alt="image" src="https://github.com/user-attachments/assets/eeaf9c94-cbca-4249-a7f1-d1b07f93dbba" />

**Question 6**
---
Write a SQL query to Delete a Specific Surgery whose ID is 3 or surgeon ID is 4.

Sample table: Surgeries
```sql
DELETE FROM surgeries
WHERE surgery_id = 3
   OR surgeon_id = 4;
```

**Output:**

<img width="1055" height="661" alt="image" src="https://github.com/user-attachments/assets/89db04f7-0a7c-4a5a-92f5-29cf43f005b0" />

**Question 7**
---
Write a SQL query to Delete all Doctors whose Specialization is either 'Pediatrics' or 'Cardiology' and Last Name is Brown.

Sample table: Doctors

attributes : doctor_id, first_name, last_name, specialization
```sql
DELETE FROM doctors
WHERE last_name = 'Brown'
  AND specialization IN ('Pediatrics', 'Cardiology');
```

**Output:**

<img width="1012" height="697" alt="image" src="https://github.com/user-attachments/assets/0a16e52b-6850-4142-af93-ce75715bdca9" />

**Question 8**
---
Write a SQL query to Delete customers from 'customer' table where 'AGENT_CODE' is either 'A003' or 'A008'.

 
Sample table: Customer

+-----------+-------------+-------------+--------------+--------------+-------+-------------+-------------+-------------+---------------+--------------+------------+  
|CUST_CODE  | CUST_NAME   | CUST_CITY   | WORKING_AREA | CUST_COUNTRY | GRADE | OPENING_AMT | RECEIVE_AMT | PAYMENT_AMT |OUTSTANDING_AMT| PHONE_NO     | AGENT_CODE |
+-----------+-------------+-------------+--------------+--------------+-------+-------------+-------------+-------------+---------------+--------------+------------+
| C00013    | Holmes      | London      | London       | UK           |     2 |     6000.00 |     5000.00 |     7000.00 |       4000.00 | BBBBBBB      | A003       |
| C00001    | Micheal     | New York    | New York     | USA          |     2 |     3000.00 |     5000.00 |     2000.00 |       6000.00 | CCCCCCC      | A008       |
| C00020    | Albert      | New York    | New York     | USA          |     3 |     500
```sql
DELETE FROM customer
WHERE agent_code IN ('A003', 'A008');
```

**Output:**

<img width="600" height="802" alt="image" src="https://github.com/user-attachments/assets/c1831606-533d-4ed1-adbe-f080ceaeffab" />

**Question 9**
---
Write a SQL query to Delete customers with following conditions

'CUST_COUNTRY' is not in a list of specified countries ('UK', 'USA', 'Canada')
'GRADE' is greater than or equal to 3
```sql
DELETE FROM customer
WHERE cust_country NOT IN ('UK', 'USA', 'Canada')
  AND grade >= 3;
```

**Output:**

<img width="1763" height="270" alt="image" src="https://github.com/user-attachments/assets/2fc90506-d131-490c-832e-bfcbf21a3c25" />

**Question 10**
---
Write a SQL query to Delete All Doctors with a NULL Specialization

Sample table: Doctors

attributes : doctor_id, first_name, last_name, specialization
```sql
DELETE FROM doctors
WHERE specialization IS NULL;
```

**Output:**

<img width="1073" height="737" alt="image" src="https://github.com/user-attachments/assets/6a22c094-d016-4771-af71-004bd87f8df7" />

## RESULT
Thus, the SQL queries to implement DML commands have been executed successfully.

<img width="1492" height="697" alt="image" src="https://github.com/user-attachments/assets/b1abe4e2-5060-457f-81e5-011fcd00bf9b" />

