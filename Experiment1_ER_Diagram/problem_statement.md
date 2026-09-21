# ER Diagram Workshop – Submission Template

## Objective
To understand and apply ER modeling concepts by creating ER diagrams for real-world applications.

## Purpose
Gain hands-on experience in designing ER diagrams that represent database structure including entities, relationships, attributes, and constraints.

---

# Scenario A: City Fitness Club Management

**Business Context:**  
FlexiFit Gym wants a database to manage its members, trainers, and fitness programs.

**Requirements:**  
- Members register with name, membership type, and start date.  
- Each member can join multiple programs (Yoga, Zumba, Weight Training).  
- Trainers assigned to programs; a program may have multiple trainers.  
- Members may book personal training sessions with trainers.  
- Attendance recorded for each session.  
- Payments tracked for memberships and sessions.

### ER Diagram:
*Paste or attach your diagram here*  
<img width="1133" height="730" alt="image" src="https://github.com/user-attachments/assets/bea0471a-33d1-4cc7-8b2c-1b1ccc7fa1f5" />


### Entities and Attributes

| Entity | Attributes (PK, FK) | Notes |
|--------|--------------------|-------|
|   Member     |     MemberID (PK), Name, Age               |     Members join programs, make payments, book sessions.  |
|   Program     |    ProgramID (PK), Name, Schedule                |    Programs have members and assigned trainers.   |
|  Trainer      |          TrainerID (PK), Name, Specialization          |   Trainers conduct sessions and take attendance.    |
|     Session   |       SessionID (PK), Date, Time, TrainerID (FK)             |   Sessions are conducted by a trainer and are bookable by members.    |
|     Payment   |         PaymentID (PK), Amount, Date, Type, MemberID (FK)           |  Members make payments.     |

### Relationships and Constraints

| Relationship | Cardinality | Participation | Notes |
|--------------|------------|---------------|-------|
|        Member — Join — Program      |     M : 1       |        Member (optional), Program (mandatory)       |   A member can join multiple programs; a program has many members.    |
|           Program — Assigned To — Trainer   |     M : 1       |       Program (mandatory), Trainer (optional)        |    Each program is handled by one trainer; a trainer can handle multiple programs.   |
|  Trainer — Conducted By — Session            |    1 : M        |           Trainer (mandatory), Session (mandatory)    |   Each session is conducted by exactly one trainer.    |

### Assumptions
---
A session is always conducted by one trainer.

Members can join zero or many programs.

A program must have a trainer assigned (based on diagram’s participation).

A member may book sessions independently of joining programs.

Attendance is only recorded for booked sessions.

Payment does not depend on programs or sessions; a member simply makes payments.

Booking relationship between Member and Session is M:N but represented implicitly through Attendance (or could be a separate link table).

---

# Scenario B: City Library Event & Book Lending System

**Business Context:**  
The Central Library wants to manage book lending and cultural events.

**Requirements:**  
- Members borrow books, with loan and return dates tracked.  
- Each book has title, author, and category.  
- Library organizes events; members can register.  
- Each event has one or more speakers/authors.  
- Rooms are booked for events and study.  
- Overdue fines apply for late returns.

### ER Diagram:
*Paste or attach your diagram here*  
<img width="822" height="570" alt="image" src="https://github.com/user-attachments/assets/8baafc0c-84c9-4cfc-b627-ac98a4aba0dc" />

### Entities and Attributes

| Entity | Attributes (PK, FK) | Notes |
|--------|--------------------|-------|
|    Member    |     MemberID (PK), Name, Email               |     A member can make loans and attend events.  |
|   Loan     |                LoanID (PK), BorrowDate, LoanExpiryDate, MemberID (FK)    |   A loan is made by one member and covers one book.    |
|       Book |             ISBN (PK), Name, Author       |     A book can be part of many loans.  |
|  Fine      |         FineID (PK), Amount, DateExceeded, LoanID (FK)           |     A fine is generated when a loan exceeds expiry.  |
|      Event  |             EventID (PK), Date, Time, Venue       |    Members attend events; events use rooms.   |

### Relationships and Constraints

| Relationship | Cardinality | Participation | Notes |
|--------------|------------|---------------|-------|
|     Member – Makes – Loan         |    1 : M        |           Member (mandatory), Loan (mandatory)    |    One member can make many loans; each loan belongs to one member.   |
|          Loan – Covers – Book    |        M : 1    |    Loan (mandatory), Book (mandatory)           | A loan covers exactly one book; a book can appear in multiple loans.      |
|      Loan – Generates – Fine        |    1 : M        |         Loan (optional), Fine (mandatory)      |      A loan may generate zero or many fines; each fine belongs to one loan. |
|     Member – Attends – Event         |    M : M        |       Both optional        |    Members can attend many events; events have many attendees.   |
|        Event – Use – Rooms      |     M : 1       |          Event (mandatory), Rooms (optional)     |      An event uses one room; a room can host many events. |
|    Rooms – Conduct – Author/Speaker          |      1 : 1      |        Both mandatory       |    One author/speaker conducts an event in exactly one room at a time.   |

### Assumptions
Each loan is strictly linked to one book (not multiple books per loan).

A member can exist without creating loans or attending events.

Fines only exist if a loan has exceeded its expiry date.

Every event must be held in a room.

Author/Speaker is treated as its own entity, not part of Member.

Each event has one speaker, and each speaker conducts one event at a time.

Room availability indicates whether the room can host events.

---

# Scenario C: Restaurant Table Reservation & Ordering

**Business Context:**  
A popular restaurant wants to manage reservations, orders, and billing.

**Requirements:**  
- Customers can reserve tables or walk in.  
- Each reservation includes date, time, and number of guests.  
- Customers place food orders linked to reservations.  
- Each order contains multiple dishes; dishes belong to categories (starter, main, dessert).  
- Bills generated per reservation, including food and service charges.  
- Waiters assigned to serve reservations.

### ER Diagram:
*Paste or attach your diagram here*  
<img width="826" height="606" alt="image" src="https://github.com/user-attachments/assets/bbe4f5d7-245a-4b45-988c-ef1e44857589" />

### Entities and Attributes

| Entity | Attributes (PK, FK) | Notes |
|--------|--------------------|-------|
|    Customer    |        CustomerID (PK), Name, PhoneNo            |   Customers make reservations and pay bills.    |
|   Reservation     |      ReservationID (PK), Date, Time, NoOfGuests, CustomerID (FK)              |  Customers create reservations.     |
|      Table  |      TableNumber (PK), Capacity              |    Tables are assigned to reservations.   |
|   Waiter     |    WaiterID (PK), Name                |   Waiters are assigned to reservations.    |
|    Order    |        OrderID (PK), Time, ReservationID (FK)            |    Orders are linked to reservations.   |

### Relationships and Constraints

| Relationship | Cardinality | Participation | Notes |
|--------------|------------|---------------|-------|
|     Customer — Makes — Reservation         |     1 : M       |        Customer (mandatory), Reservation (mandatory)       |    A customer can make many reservations.   |
|      Customer — Pays — Bill        |      1 : 1      |      Both mandatory         |   One bill per customer per reservation.    |
|        Reservation — Assigned — Table      |      1 : M      |     Reservation (mandatory), Table (mandatory)          |   One reservation uses exactly one table; a table can be used by many reservations over time.   |

### Assumptions
A reservation must be linked to a customer, table, and waiter.

A customer pays exactly one bill per reservation.

A dish cannot exist without belonging to a category.

A reservation can have zero or many orders.

Orders must belong to a reservation.

A table may be assigned to many reservations over time, but one reservation gets only one table.

Category names like starter/main/dessert are treated as attribute values, not entities.

---

### RESULT
The experiment has been executed successfully and the ER diagrams were designed for all the given scenarios. The entities, attributes, relationships, and constraints were identified correctly. Thus, the database models were created successfully for the real-world applications

## Instructions for Students

1. Complete **all three scenarios** (A, B, C).  
2. Identify entities, relationships, and attributes for each.  
3. Draw ER diagrams using **draw.io / diagrams.net** or hand-drawn & scanned.  
4. Fill in all tables and assumptions for each scenario.  
5. Export the completed Markdown (with diagrams) as **a single PDF**
