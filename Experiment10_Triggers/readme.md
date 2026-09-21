# Experiment 10: PL/SQL – Triggers

## AIM
To write and execute PL/SQL trigger programs for automating actions in response to specific table events like INSERT, UPDATE, or DELETE.

---

## THEORY

A **trigger** is a stored PL/SQL block that is automatically executed or fired when a specified event occurs on a table or view. Triggers can be used for enforcing business rules, auditing changes, or automatic updates.

### Types of Triggers:
- **Before Trigger**: Executes before the operation (INSERT, UPDATE, DELETE).
- **After Trigger**: Executes after the operation.
- **Row-level Trigger**: Executes for each affected row.
- **Statement-level Trigger**: Executes once for the triggering statement.

**Basic Syntax:**
```sql
CREATE OR REPLACE TRIGGER trigger_name
BEFORE|AFTER INSERT|UPDATE|DELETE ON table_name
[FOR EACH ROW]
BEGIN
   -- trigger logic
END;
```

## 1. Write a trigger to log every insertion into a table.
**Steps:**
- Create two tables: `employees` (for storing data) and `employee_log` (for logging the inserts).
- Write an **AFTER INSERT** trigger on the `employees` table to log the new data into the `employee_log` table.

**Expected Output:**
- A new entry is added to the `employee_log` table each time a new record is inserted into the `employees` table.

## Query:
```sql
CREATE OR REPLACE TRIGGER trg_log_employee_insert
AFTER INSERT ON employees
FOR EACH ROW
BEGIN
   INSERT INTO employee_log (emp_id, emp_name, action_time)
   VALUES (:NEW.emp_id, :NEW.emp_name, SYSDATE);
END;
CREATE TABLE employee_log (
   emp_id     NUMBER,
   emp_name   VARCHAR2(50),
   action_time DATE
);
CREATE OR REPLACE TRIGGER trg_log_employee_insert
AFTER INSERT ON employees
FOR EACH ROW
BEGIN
   INSERT INTO employee_log (emp_id, emp_name, action_time)
   VALUES (:NEW.emp_id, :NEW.emp_name, SYSDATE);
END;
INSERT INTO employees VALUES (201, 'Ravi', 'Intern', 3500, 40);
SELECT * FROM employee_log;

DEVELOPED BY: JINITH KUMAR V
REGISTER NO: 212225040157
```

<img width="762" height="306" alt="image" src="https://github.com/user-attachments/assets/f1e76029-02ec-414b-9870-d107e693f610" />

---

## 2. Write a trigger to prevent deletion of records from a sensitive table.
**Steps:**
- Write a **BEFORE DELETE** trigger on the `sensitive_data` table.
- Use `RAISE_APPLICATION_ERROR` to prevent deletion and issue a custom error message.

**Expected Output:**
- If an attempt is made to delete a record from `sensitive_data`, an error message is raised, e.g., `ERROR: Deletion not allowed on this table.`

## Query:
```sql
DROP TABLE sensitive_data CASCADE CONSTRAINTS;
CREATE TABLE sensitive_data (
    id NUMBER PRIMARY KEY,
    data VARCHAR2(100)
);
INSERT INTO sensitive_data (id, data)
VALUES (1, 'Confidential Data');

COMMIT;
CREATE OR REPLACE TRIGGER prevent_sensitive_delete
BEFORE DELETE ON sensitive_data
FOR EACH ROW
BEGIN
    RAISE_APPLICATION_ERROR(
        -20001,
        'Deletion not allowed on this table.'
    );
END;
/
DELETE FROM sensitive_data
WHERE id = 1;
```

<img width="761" height="290" alt="image" src="https://github.com/user-attachments/assets/65bc82e9-1baa-40d0-a809-3d03511c9b03" />

---

## 3. Write a trigger to automatically update a `last_modified` timestamp.
**Steps:**
- Add a `last_modified` column to the `products` table.
- Write a **BEFORE UPDATE** trigger on the `products` table to set the `last_modified` column to the current timestamp whenever an update occurs.

**Expected Output:**
- The `last_modified` column in the `products` table is updated automatically to the current date and time when any record is updated.

## Query:
```
CREATE TABLE products (
    product_id NUMBER PRIMARY KEY,
    product_name VARCHAR2(50),
    price NUMBER
);
INSERT INTO products (product_id, product_name, price)
VALUES (101, 'Laptop', 50000);

COMMIT;
ALTER TABLE products
ADD last_modified TIMESTAMP;
CREATE OR REPLACE TRIGGER update_last_modified
BEFORE UPDATE ON products
FOR EACH ROW
BEGIN
    :NEW.last_modified := SYSTIMESTAMP;
END;
/
UPDATE products
SET price = 55000
WHERE product_id = 101;

COMMIT;
SELECT * FROM products;
```

<img width="713" height="245" alt="image" src="https://github.com/user-attachments/assets/88d2a116-ed88-452a-a48b-d80f6b280e15" />

---

## 4. Write a trigger to keep track of the number of updates made to a table.
**Steps:**
- Create an `audit_log` table with a counter column.
- Write an **AFTER UPDATE** trigger on the `customer_orders` table to increment the counter in the `audit_log` table every time a record is updated.

**Expected Output:**
- The `audit_log` table will maintain a count of how many updates have been made to the `customer_orders` table.

## Query:
```
CREATE TABLE customer_orders (
    order_id NUMBER PRIMARY KEY,
    customer_name VARCHAR2(50),
    amount NUMBER
);
INSERT INTO customer_orders (order_id, customer_name, amount)
VALUES (1, 'Rifana', 1000);

COMMIT;
CREATE TABLE audit_log (
    id NUMBER PRIMARY KEY,
    update_count NUMBER
);
INSERT INTO audit_log (id, update_count)
VALUES (1, 0);

COMMIT;
CREATE OR REPLACE TRIGGER count_customer_updates
AFTER UPDATE ON customer_orders
FOR EACH ROW
BEGIN
    UPDATE audit_log
    SET update_count = update_count + 1
    WHERE id = 1;
END;
/
UPDATE customer_orders
SET amount = 1500
WHERE order_id = 1;

COMMIT;
SELECT * FROM audit_log;
UPDATE customer_orders
SET amount = 2000
WHERE order_id = 1;

COMMIT;
SELECT * FROM audit_log;
```

<img width="770" height="267" alt="image" src="https://github.com/user-attachments/assets/c72e3b61-0844-4b5f-a5d0-17218b4aac77" />

---

## 5. Write a trigger that checks a condition before allowing insertion into a table.
**Steps:**
- Write a **BEFORE INSERT** trigger on the `employees` table to check if the inserted salary meets a specific condition (e.g., salary must be greater than 3000).
- If the condition is not met, raise an error to prevent the insert.

**Expected Output:**
- If the inserted salary in the `employees` table is below the condition (e.g., salary < 3000), the insert operation is blocked, and an error message is raised, such as: `ERROR: Salary below minimum threshold.`

 ## Query:
```
  CREATE TABLE employees (
    emp_id NUMBER PRIMARY KEY,
    emp_name VARCHAR2(50),
    salary NUMBER
);
CREATE OR REPLACE TRIGGER check_employee_salary
BEFORE INSERT ON employees
FOR EACH ROW
BEGIN
    IF :NEW.salary < 3000 THEN
        RAISE_APPLICATION_ERROR(
            -20002,
            'Salary below minimum threshold.'
        );
    END IF;
END;
/
INSERT INTO employees (emp_id, emp_name, salary)
VALUES (101, 'Rifana', 5000);

COMMIT;
INSERT INTO employees (emp_id, emp_name, salary)
VALUES (102, 'Test', 2000);
```

<img width="690" height="283" alt="image" src="https://github.com/user-attachments/assets/876bfcb1-ef9e-4733-8b93-eeb3d7719c04" />

---
## RESULT
Thus, the PL/SQL trigger programs were written and executed successfully.
