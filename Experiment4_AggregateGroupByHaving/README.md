# Experiment 4: Aggregate Functions, Group By and Having Clause

## AIM
To study and implement aggregate functions, GROUP BY, and HAVING clause with suitable examples.

## THEORY

### Aggregate Functions
These perform calculations on a set of values and return a single value.

- **MIN()** – Smallest value  
- **MAX()** – Largest value  
- **COUNT()** – Number of rows  
- **SUM()** – Total of values  
- **AVG()** – Average of values

**Syntax:**
```sql
SELECT AGG_FUNC(column_name) FROM table_name WHERE condition;
```
### GROUP BY
Groups records with the same values in specified columns.
**Syntax:**
```sql
SELECT column_name, AGG_FUNC(column_name)
FROM table_name
GROUP BY column_name;
```
### HAVING
Filters the grouped records based on aggregate conditions.
**Syntax:**
```sql
SELECT column_name, AGG_FUNC(column_name)
FROM table_name
GROUP BY column_name
HAVING condition;
```

**Question 1**
--
How many appointments are scheduled in each hour of the day?

Sample table:Appointments Table

name                              type
--------------------          ----------
AppointmentID               INTEGER
PatientID                         INTEGER
DoctorID                         INTEGER
AppointmentDateTime   DATETIME
Purpose                           TEXT
Status                              TEXT     

```sql
SELECT strftime('%H', AppointmentDateTime) AS HourOfDay,
       COUNT(*) AS TotalAppointments
FROM Appointments
GROUP BY strftime('%H', AppointmentDateTime)
ORDER BY HourOfDay;
```

**Output:**

<img width="470" height="310" alt="image" src="https://github.com/user-attachments/assets/eb3a1752-d556-4f6d-810f-c35d1b92c2e9" />

**Question 2**
---
Write a SQL Query to find how many medications are prescribed for each patient?

Sample table:MedicalRecords Table

```sql
SELECT PatientID,
       COUNT(*) AS AvgMedications
FROM MedicalRecords
GROUP BY PatientID;
```

**Output:**

<img width="581" height="350" alt="image" src="https://github.com/user-attachments/assets/d24c0643-a1e4-4622-8f79-70231c0218f4" />

**Question 3**
---
How many appointments are scheduled for each doctor?

Sample table:Appointments Table



For example:

Result
DoctorID    TotalAppointments
----------  -----------------
3           3
4           2
6           1
7           3
10          1

```sql
SELECT DoctorID,
       COUNT(*) AS TotalAppointments
FROM Appointments
GROUP BY DoctorID;
```

**Output:**

<img width="618" height="362" alt="image" src="https://github.com/user-attachments/assets/19a26c63-0b59-4533-8297-fece5a35c20e" />

**Question 4**
---
Write a SQL query to find the maximum purchase amount.

Sample table: orders

ord_no      purch_amt   ord_date    customer_id  salesman_id

----------  ----------  ----------  -----------  -----------

70001       150.5       2012-10-05  3005         5002

70009       270.65      2012-09-10  3001         5005

70002       65.26       2012-10-05  3002         5001

 

For example:

Result
MAXIMUM
----------
5760.0

```sql
SELECT MAX(purch_amt) AS MAXIMUM
FROM orders;
```

**Output:**

<img width="451" height="207" alt="image" src="https://github.com/user-attachments/assets/cbb3175c-d584-4a5e-9166-2033dd877880" />

**Question 5**
---
Write a SQL query to find the average length of email addresses (in characters):

Table: customer

name        type
----------  ----------
id          INTEGER
name        TEXT
city        TEXT
email       TEXT
phone       INTEGER
For example:

Result
avg_email_length
----------------
15.0

```sql
SELECT AVG(LENGTH(email)) AS avg_email_length
FROM customer;
```

**Output:**

<img width="603" height="197" alt="image" src="https://github.com/user-attachments/assets/c6a73610-99d0-45f4-85d6-ab2b8bb91ae4" />

**Question 6**
---
Write a SQL query to find the difference between the maximum and minimum price of fruits?

Table: fruits

name        type
----------  ----------
id          INTEGER
name        TEXT
unit        TEXT
inventory   INTEGER
price       REAL
 

For example:

Result
price_diff
----------
4.65

```sql
SELECT MAX(price) - MIN(price) AS price_diff
FROM fruits;
```

**Output:**

<img width="627" height="206" alt="image" src="https://github.com/user-attachments/assets/9eff73bc-f248-43e8-bf8e-94889057999f" />

**Question 7**
---
Write a SQL query to find the total amount of fruits with a unit type of 'LB'.

Note: Inventory attribute contains amount of fruits

Table: fruits

name        type
----------  ----------
id          INTEGER
name        TEXT
unit        TEXT
inventory   INTEGER
price       REAL
 

For example:

Result
total
----------
225

```sql
SELECT SUM(inventory) AS total
FROM fruits
WHERE unit = 'LB';
```

**Output:**

<img width="462" height="198" alt="image" src="https://github.com/user-attachments/assets/e6721e77-8b63-4dd6-a42a-d62ed6956457" />

**Question 8**
---
Write an SQL query that groups the customer data into 5-year age intervals, calculates the minimum salary for each group, and excludes groups where the minimum salary is not less than 2000.

Table: customer1



For example:

Result
age_group   MIN(salary)
----------  -----------
25          1500

```sql
SELECT (age / 5) * 5 AS age_group,
       MIN(salary)
FROM customer1
GROUP BY (age / 5) * 5
HAVING MIN(salary) < 2000;
```

**Output:**

<img width="508" height="206" alt="image" src="https://github.com/user-attachments/assets/f36ac3f5-0dc8-4d99-918c-11ad11ee49d8" />

**Question 9**
---
Write the SQL query that achieves the grouping of data by age intervals using the expression (age/5)5, calculates the total salary sum for each group, and excludes groups where the total salary sum is not greater than 5000.

Sample table: customer1



For example:

Result
age_group   SUM(salary)
----------  -----------
20          16500
25          16500

```sql
SELECT (age / 5) * 5 AS age_group,
       SUM(salary)
FROM customer1
GROUP BY (age / 5) * 5
HAVING SUM(salary) > 5000;
```

**Output:**

<img width="580" height="230" alt="image" src="https://github.com/user-attachments/assets/2821acf2-63b6-47c3-a305-9d83229529ea" />

**Question 10**
---
Write the SQL query that accomplishes the grouping of data by age intervals using the expression (age/5)5, calculates the minimum age for each group, and excludes groups where the minimum age is not less than 25.

Sample table: customer1



For example:

Result
age_group   MIN(age)
----------  ----------
20          22

```sql
SELECT (age / 5) * 5 AS age_group,
       MIN(age)
FROM customer1
GROUP BY (age / 5) * 5
HAVING MIN(age) < 25;
```

**Output:**

<img width="625" height="206" alt="image" src="https://github.com/user-attachments/assets/09e2a383-50e1-4c82-a149-22fc54729393" />


## RESULT
Thus, the SQL queries to implement aggregate functions, GROUP BY, and HAVING clause have been executed successfully.

<img width="1493" height="705" alt="image" src="https://github.com/user-attachments/assets/1ad4120e-5ed1-4ea3-8a06-f90db727d4fd" />

