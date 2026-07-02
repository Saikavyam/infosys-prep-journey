
# Day 1 - Access Modifiers, Inheritance, and SQL

## Why This Topic Matters

Access modifiers and inheritance are foundational OOP concepts in Java. They are important not just for theory questions, but also for:
- Output-based Java MCQs
- Error spotting questions
- Understanding encapsulation
- Understanding how parent-child classes work
- Learning advanced topics like polymorphism, overriding, Spring Boot class design, and framework structure

---

# Part 1: Access Modifiers

## Definition

Access modifiers control the visibility of class members.

Class members include:
- Variables / fields
- Methods
- Constructors
- Classes (in some cases)

In simple words, access modifiers decide:

> **Who can access what?**

## Why Access Modifiers Are Needed

Imagine a banking application:

```java
class BankAccount {
    String accountHolder;
    double balance;
}
```

If balance is openly accessible, any class can do:

```java
account.balance = -5000;
```

or

```java
account.balance = 999999999;
```

This is dangerous. So Java provides access modifiers to:
- Protect data
- Restrict visibility
- Control how objects are used
- Support encapsulation

## Types of Access Modifiers in Java

Java has 4 access levels:
1. `private`
2. `default` / package-private (no keyword)
3. `protected`
4. `public`

### 1. `private`

A private member can be accessed only inside the same class.

**Example:**

```java
class BankAccount {
    private double balance;
}
```

Inside the same class, it is accessible:

```java
class BankAccount {
    private double balance;
    
    void showBalance() {
        System.out.println(balance);
    }
}
```

Outside the class, it is not accessible:

```java
BankAccount b = new BankAccount();
b.balance = 1000;   // Error
```

**Why private is important:**

Private is the backbone of encapsulation. Important internal data should not be modified directly from outside the class.

Instead of doing this:

```java
b.balance = -5000;
```

We should provide controlled methods like:
- `deposit()`
- `withdraw()`
- `getBalance()`

**Real-world use of private:**
- Banking app: `balance`, `ATM pin`, `transaction history`
- E-commerce app: `stock quantity`, `payment status`, `internal discount logic`
- Student portal / HR systems: `marks`, `salary`, `attendance records`

### 2. Default Access Modifier (Package-Private)

If no modifier is written, Java gives default access.

**Example:**

```java
class Student {
    int age;  // default access
}
```

**Meaning:** A default member is accessible:
- Inside the same class
- Inside the same package
- Not accessible outside the package

**Why default access exists:**

Sometimes a member does not need to be public for the whole world, but it can still be shared among closely related classes in the same package.

### 3. `protected`

**Definition:** A protected member is accessible:
- Inside the same class
- Inside the same package
- In child classes (subclasses)

**Example:**

```java
class Employee {
    protected double salary;
}
```

If another class extends Employee, it can access salary.

**Why protected exists:**

Sometimes a parent class wants to share certain data or methods with its child classes, but not expose them to everyone.

For example:
- Parent class: `Vehicle`
- Child classes: `Car`, `Bike`, `Truck`

A child class may need access to some parent-level fields or methods, but unrelated external classes should not.

### 4. `public`

**Definition:** A public member is accessible from anywhere.

**Example:**

```java
public class Student {
    public String name;
}
```

If a class is visible/imported properly, any class can access a public member.

**Real-world use of public:**

Methods that are part of the official API of a class are often public. Examples:
- `deposit()`
- `withdraw()`
- `getBalance()`
- `startEngine()`
- `displayProfile()`

## Access Modifier Visibility Table

> **This table is important. Memorize it.**

| Modifier | Same Class | Same Package | Child Class | Everywhere |
|----------|------------|--------------|-------------|------------|
| private  | Yes        | No           | No          | No         |
| default  | Yes        | Yes          | No*         | No         |
| protected| Yes        | Yes          | Yes         | No         |
| public   | Yes        | Yes          | Yes         | Yes        |

*Default does not give special access to child classes outside the package.

## Proper Use of Access Modifiers

### Bad Design

```java
class BankAccount {
    public double balance;
}
```

**Problem:**
- Any class can change balance directly
- Invalid values can be assigned
- No validation is possible

### Better Design

```java
class BankAccount {
    private double balance;
    
    public void deposit(double amount) {
        if (amount > 0) {
            balance += amount;
        }
    }
    
    public void withdraw(double amount) {
        if (amount > 0 && amount <= balance) {
            balance -= amount;
        }
    }
    
    public double getBalance() {
        return balance;
    }
}
```

**Advantages:**
- Data is protected
- Operations are controlled
- Validation is possible
- Object state remains safe

---

# Part 2: Inheritance

## Definition

Inheritance is the mechanism by which one class acquires the properties and behaviors of another class.

In Java, inheritance is implemented using the keyword: `extends`

### Example

```java
class Vehicle {
    String brand;
    
    void start() {
        System.out.println("Vehicle started");
    }
}

class Car extends Vehicle {
    void playMusic() {
        System.out.println("Music playing");
    }
}
```

Now Car can use:
- `brand`
- `start()`
- `playMusic()`

## Parent and Child Class Terminology

If:

```java
class Vehicle { }
class Car extends Vehicle { }
```

Then:
- **Parent class** / superclass / base class = `Vehicle`
- **Child class** / subclass / derived class = `Car`

## Why Inheritance Exists

Inheritance exists mainly to avoid duplication and model real-world relationships.

Suppose we have:
- Car
- Bike
- Truck

All of them have:
- brand
- speed
- start()
- stop()

Without inheritance, we would write the same code repeatedly in multiple classes.

Inheritance allows us to keep common properties in one parent class and reuse them in child classes.

## Core Idea of Inheritance

If a child class extends a parent class, the child can use the parent's accessible members.

**Example:**

```java
class Animal {
    String name;
    
    void eat() {
        System.out.println("Animal is eating");
    }
}

class Dog extends Animal {
    void bark() {
        System.out.println("Dog is barking");
    }
}
```

**Usage:**

```java
Dog d = new Dog();
d.name = "Bruno";
d.eat();
d.bark();
```

**Output:**
```
Animal is eating
Dog is barking
```

## IS-A Relationship

Inheritance should be used only when there is a true IS-A relationship.

**Good examples:**
- Car **is a** Vehicle ✓
- Dog **is an** Animal ✓
- SavingsAccount **is a** BankAccount ✓
- Manager **is an** Employee ✓

**Bad examples:**
- Engine **is a** Car ✗
- Address **is a** Student ✗
- Customer **is a** Bank ✗

## Real-World Applications of Inheritance

### Banking System

**Parent:** `BankAccount`

**Child classes:** `SavingsAccount`, `CurrentAccount`, `SalaryAccount`

**Common data:**
- account number
- account holder
- balance
- deposit()
- withdraw()

**Specific child data:**
- interest rate for savings account
- overdraft limit for current account

### E-commerce System

**Parent:** `Product`

**Child classes:** `Book`, `Electronics`, `Clothing`

**Common:** productId, name, price

**Specific:**
- author for books
- warranty for electronics
- size for clothing

### HR Software

**Parent:** `Employee`

**Child classes:** `Developer`, `Manager`, `HR`

**Common:** employeeId, name, salary

**Specific:**
- programmingLanguage for Developer
- teamSize for Manager

## What Gets Inherited?

A child class can inherit:
- Non-private variables
- Non-private methods

A child class cannot directly access private members of the parent.

**Example:**

```java
class Parent {
    private int x = 10;
}

class Child extends Parent {
    // Child does not get direct access to x
}
```

## Inheritance and Access Modifiers Together

**Example:**

```java
class Employee {
    private int id;
    protected double salary;
    public String name;
}
```

In a child class:
- `id` cannot be directly accessed
- `salary` can be accessed
- `name` can be accessed

This is why `protected` is especially relevant in inheritance-based code.

## Types of Inheritance in Java

### 1. Single Inheritance
One child inherits from one parent.

```java
class A { }
class B extends A { }
```

### 2. Multilevel Inheritance
A chain of inheritance.

```java
class Animal { }
class Dog extends Animal { }
class Puppy extends Dog { }
```

Here Puppy indirectly gets features from Animal.

### 3. Hierarchical Inheritance
Multiple child classes inherit from one parent.

```java
class Vehicle { }
class Car extends Vehicle { }
class Bike extends Vehicle { }
class Truck extends Vehicle { }
```

### 4. Multiple Inheritance of Classes
Java does not support multiple inheritance using classes.

```java
// This is invalid:
class C extends A, B { }
```

**Reason:** Ambiguity problems / diamond problem.

Java handles multiple-type behavior using interfaces.

## Constructor Behavior in Inheritance

When a child object is created, the parent constructor executes first.

**Example:**

```java
class Parent {
    Parent() {
        System.out.println("Parent constructor");
    }
}

class Child extends Parent {
    Child() {
        System.out.println("Child constructor");
    }
}
```

```java
Child c = new Child();
```

**Output:**
```
Parent constructor
Child constructor
```

**Why does this happen?**

A child object contains the parent portion also. So when Java creates a child object, it first initializes the parent part and then the child part.

## Method Overriding - Preview

Sometimes a child class wants to provide its own version of a parent method.

**Example:**

```java
class Animal {
    void sound() {
        System.out.println("Animal makes sound");
    }
}

class Dog extends Animal {
    @Override
    void sound() {
        System.out.println("Dog barks");
    }
}
```

This is called **method overriding**.

## When NOT to Use Inheritance

Do not use inheritance just because two classes share some fields.

Use inheritance only when:
- There is a genuine IS-A relationship
- Child is a specialized version of parent
- Common behavior is meaningful to share

**Bad use cases:**
- Engine and Car
- Address and Student
- Keyboard and Computer

These are better represented using **composition** / has-a relationship.

## Key Differences: Encapsulation vs Inheritance

### Encapsulation
- Hide data
- Expose controlled methods
- Protect internal state

**Example:** `private balance`, `public deposit()`

### Inheritance
- One class reuses and extends another class
- Common behavior is shared through parent-child relationship

**Example:** `Car extends Vehicle`

---

# Part 3: SQL Basics

## Why This Topic Matters

SQL is one of the most important parts of Infosys training and pretests. Even if the SQL questions become moderate or advanced later, the advanced part is built on these basic concepts.

If these basics are weak, then:
- JOINs become confusing
- GROUP BY and HAVING become messy
- Subqueries become painful
- Large-table questions become overwhelming

So this first layer must be clean.

## What is Data?

Data means raw facts or information.

**Examples:**
- Employee name
- Student roll number
- Department name
- Salary
- Phone number
- Order id

If a company like Infosys has 50,000 employees, all their names, ids, departments, salaries, project details etc. are data.

## What is a Database?

A database is an organized collection of data.

**Example:** An employee database may store records like this:

| EmpID | Name | Department | Salary |
|-------|------|------------|--------|
| 101 | Sai | AI | 50000 |
| 102 | Rahul | Java | 70000 |
| 103 | Priya | Data | 65000 |

Instead of storing this data randomly in notebooks or multiple disconnected files, a database stores it in an organized way so that it can be searched, updated, and managed efficiently.

## What is DBMS?

**DBMS = Database Management System**

A DBMS is software used to create, manage, store, retrieve, and manipulate data in a database.

**Examples of DBMS / database systems:**
- MySQL
- Oracle
- PostgreSQL
- SQL Server

A DBMS allows us to:
- Create tables
- Insert records
- Update records
- Delete records
- Retrieve records
- Manage large amounts of data safely

## What is RDBMS?

**RDBMS = Relational Database Management System**

An RDBMS stores data in the form of **tables** and allows relationships between tables.

**Example:**

### Employee Table

| EmpID | Name | DeptID |
|-------|------|--------|
| 1 | Sai | 10 |
| 2 | Rahul | 20 |

### Department Table

| DeptID | DeptName |
|--------|----------|
| 10 | AI |
| 20 | Java |

Here the `DeptID` in Employee table is related to the `DeptID` in Department table. That is why it is called a **relational** database system.

## Table, Row, and Column

### Table
A table is a collection of related data arranged in rows and columns.

**Example:** `Employee` table.

### Row
A row represents one complete record.

**Example:** `1 | Sai | AI | 50000`

### Column
A column represents one attribute / field of the data.

**Examples:**
- EmpID
- Name
- Department
- Salary

## Primary Key

A primary key is a column (or a set of columns) that uniquely identifies each row in a table.

**Example:**

| EmpID | Name |
|-------|------|
| 101 | Sai |
| 102 | Rahul |

Here `EmpID` can be the primary key because it uniquely identifies each employee.

**Properties of primary key:**
- Unique
- Not null
- One primary key per table (though it can be composite)

## Foreign Key

A foreign key is a column in one table that refers to the primary key of another table.

**Example:**

### Department Table

| DeptID | DeptName |
|--------|----------|
| 10 | AI |
| 20 | Java |

### Employee Table

| EmpID | Name | DeptID |
|-------|------|--------|
| 1 | Sai | 10 |
| 2 | Rahul | 20 |

Here `Employee.DeptID` is a foreign key referencing `Department.DeptID`.

**Why foreign keys are important:**

Foreign keys create relationships between tables and help maintain consistency.

**Examples:**
- An employee's department should ideally exist in the Department table
- Orders should refer to valid customers
- Students should refer to valid courses or branches

## What is SQL?

**SQL = Structured Query Language**

SQL is the language used to communicate with relational databases.

Using SQL, we can:
- Retrieve data
- Filter data
- Sort data
- Count records
- Insert new records
- Update records
- Delete records

## SELECT

`SELECT` is used to retrieve data from a table.

> **SELECT = show me data**

### Syntax

```sql
SELECT column_name
FROM table_name;

-- or

SELECT *
FROM table_name;
```

### Example Table: Employee

| EmpID | Name | Department | Salary |
|-------|------|------------|--------|
| 1 | Sai | AI | 50000 |
| 2 | Rahul | Java | 70000 |
| 3 | Priya | AI | 60000 |
| 4 | Arjun | Data | 80000 |

### Example 1 - Show all columns

```sql
SELECT * FROM Employee;
```

### Example 2 - Show selected columns

```sql
SELECT Name, Salary
FROM Employee;
```

## WHERE

`WHERE` is used to filter rows based on a condition.

> **WHERE = keep only the rows that satisfy the condition**

### Example 1

```sql
SELECT *
FROM Employee
WHERE Department = 'AI';
```

### Example 2

```sql
SELECT Name, Salary
FROM Employee
WHERE Salary > 55000;
```

## DISTINCT

`DISTINCT` is used to remove duplicate values from the result.

```sql
SELECT DISTINCT Department
FROM Employee;
```

**Output:**
```
AI
Java
Data
```

## ORDER BY

`ORDER BY` is used to sort the result.

### Ascending Order

```sql
SELECT *
FROM Employee
ORDER BY Salary;
```

or explicitly:

```sql
SELECT *
FROM Employee
ORDER BY Salary ASC;
```

### Descending Order

```sql
SELECT *
FROM Employee
ORDER BY Salary DESC;
```

## COUNT()

`COUNT()` is used to count rows / records.

### Count All Rows

```sql
SELECT COUNT(*)
FROM Employee;
```

### Count with a Condition

```sql
SELECT COUNT(*)
FROM Employee
WHERE Department = 'AI';
```

## How to Read a SQL Query in English

This is very important for big-table questions.

**Example:**

```sql
SELECT Name, Salary
FROM Employee
WHERE Salary > 50000
ORDER BY Salary DESC;
```

**Read it step by step:**
1. Start with the Employee table
2. Keep only rows where salary > 50000
3. From those rows, show only Name and Salary
4. Sort the result by salary in descending order

If you learn to translate queries into plain English, SQL becomes much easier.

## Difference Between DBMS and RDBMS

| Feature | DBMS | RDBMS |
|---------|------|-------|
| Full form | Database Management System | Relational Database Management System |
| Data storage | Can store data in different forms | Stores data in tables |
| Relationships | May or may not support relations | Supports relations between tables |
| Example | general DB systems | MySQL, Oracle, PostgreSQL, SQL Server |

## Important Syntax Notes

### 1. String values should be in single quotes

**Correct:**
```sql
WHERE Department = 'AI'
```

**Avoid:**
```sql
WHERE Department = "AI"
```

### 2. Table names and column names must be consistent

If the table is `Employee`, don't write:
- `Employees`
- `EMPLOYEES`
- `EmployeeData`

Unless the actual table name is that.

### 3. SQL keyword spelling matters

**Examples of common mistakes:**
- `SELEF` instead of `SELECT`
- `SELLET` instead of `SELECT`

---

# Practice Assignments

## Part A - OOP Assignment

### Question

Create a Java class named `BankAccount` with the following requirements:

### Fields
- `private String ownerName`
- `private double balance`

### Constructor
- `BankAccount(String ownerName, double balance)`

### Methods
- `deposit(double amount)`
- `withdraw(double amount)`
- `getBalance()`

### Rules
- Deposit should happen only if `amount > 0`
- Withdrawal should happen only if `amount > 0` and `amount <= balance`

### Correct Answer

```java
class BankAccount {
    private String ownerName;
    private double balance;
    
    public BankAccount(String ownerName, double balance) {
        this.ownerName = ownerName;
        this.balance = balance;
    }
    
    public void deposit(double amount) {
        if (amount > 0) {
            balance += amount;
        }
    }
    
    public void withdraw(double amount) {
        if (amount > 0 && amount <= balance) {
            balance -= amount;
        }
    }
    
    public double getBalance() {
        return balance;
    }
}
```

### Concept Check

This assignment tests:
- Class creation
- Private fields
- Constructor creation
- `this` keyword
- Encapsulation
- Method writing
- Basic validation logic

## Part B - SQL Assignment

### Assumed Table: Employee

| EmpID | Name | Department | Salary |
|-------|------|------------|--------|
| 1 | Sai | AI | 50000 |
| 2 | Rahul | Java | 70000 |
| 3 | Priya | AI | 60000 |
| 4 | Arjun | Data | 80000 |

### Questions

1. Show all employees
2. Show only Name and Salary
3. Show only AI department employees
4. Show employees with salary > 60000
5. Show unique departments
6. Sort salary ascending
7. Sort salary descending
8. Count total employees

### Correct Answers

#### 1. Show all employees

```sql
SELECT * FROM Employee;
```

#### 2. Show only Name and Salary

```sql
SELECT Name, Salary
FROM Employee;
```

#### 3. Show only AI department employees

```sql
SELECT *
FROM Employee
WHERE Department = 'AI';
```

#### 4. Show employees with salary > 60000

```sql
SELECT *
FROM Employee
WHERE Salary > 60000;
```

#### 5. Show unique departments

```sql
SELECT DISTINCT Department
FROM Employee;
```

#### 6. Sort salary ascending

```sql
SELECT Salary
FROM Employee
ORDER BY Salary;
```

**Alternative acceptable answer:**

```sql
SELECT *
FROM Employee
ORDER BY Salary;
```

#### 7. Sort salary descending

```sql
SELECT Salary
FROM Employee
ORDER BY Salary DESC;
```

**Alternative acceptable answer:**

```sql
SELECT *
FROM Employee
ORDER BY Salary DESC;
```

#### 8. Count total employees

```sql
SELECT COUNT(*)
FROM Employee;
```

---

# Mistake Log - Common Errors

## Java Mistakes Made

### 1. Wrong field declaration

**Incorrect:**
```java
private String double balance;
```

**Correct:**
```java
private double balance;
```

**Reason:** A variable can have only one data type.

### 2. Wrong return type for deposit()

**Incorrect:**
```java
public double deposit(double amount)
```

**Correct:**
```java
public void deposit(double amount)
```

**Reason:** `deposit()` only performs an action. It does not return a value in this assignment.

### 3. Wrong return type for withdraw()

**Incorrect:**
```java
public double withdraw(double amount)
```

**Correct:**
```java
public void withdraw(double amount)
```

**Reason:** `withdraw()` only updates the balance. It does not return a value here.

### 4. Typo in getter name

**Incorrect:**
```java
gteBalance()
```

**Correct:**
```java
getBalance()
```

### 5. Naming inconsistency risk

Use `ownerName` consistently instead of mixing forms like:
- `OwnerName`
- `ownerName`

## SQL Mistakes Made

### 1. Table name inconsistency

**Incorrect:**
```sql
SELECT * FROM Employees;
```

**Correct:**
```sql
SELECT * FROM Employee;
```

**Reason:** Table names must match exactly.

### 2. Column name inconsistency

**Incorrect:**
```sql
SELECT DISTINCT Departments FROM Employee;
```

**Correct:**
```sql
SELECT DISTINCT Department FROM Employee;
```

**Reason:** The actual column name is `Department`, not `Departments`.

### 3. SQL keyword typo

**Incorrect:**
```sql
SELEF
SELLET
```

**Correct:**
```sql
SELECT
```

### 4. Narrow answer to a broader question

**Question:** Show only AI department employees

**Incorrect narrow answer:**
```sql
SELECT Salary
FROM Employee
WHERE Department = 'AI';
```

**Better answer:**
```sql
SELECT *
FROM Employee
WHERE Department = 'AI';
```

**Reason:** The question asked for employees from AI department, not only their salaries.

---

# Summary / Revision Notes

## Access Modifiers

| Modifier | Access Level |
|----------|--------------|
| private | Only inside the same class |
| default | Within the same package |
| protected | Within the same package + child classes |
| public | Everywhere |

## Inheritance

- **Definition:** One class acquires properties and behaviors of another
- **Keyword:** `extends`
- **Benefits:** Code reuse, avoids duplication, creates logical class hierarchy
- **Important rule:** Use inheritance only for a true IS-A relationship
- **Types in Java:** Single, Multilevel, Hierarchical
- **Not supported:** Multiple inheritance of classes
- **Constructor rule:** Parent constructor runs before child constructor

## SQL

- **Database:** Organized collection of data
- **DBMS:** Software to manage databases
- **RDBMS:** Database system with related tables
- **SELECT:** Retrieve data
- **WHERE:** Filter rows
- **DISTINCT:** Remove duplicates
- **ORDER BY:** Sort rows
- **COUNT():** Count rows

## Quick Memory Sheet

### Java
```
private   -> only same class
default   -> same package
protected -> same package + child class
public    -> everywhere

Inheritance = child class extends parent class
Use inheritance only for IS-A relationship
Child gets accessible members of parent
Private members are not directly accessible in child
Parent constructor runs before child constructor
```

### SQL
```
Database -> organized collection of data
DBMS -> software to manage databases
RDBMS -> database system with related tables

SELECT   -> retrieve data
WHERE    -> filter rows
DISTINCT -> remove duplicates
ORDER BY -> sort rows
COUNT()  -> count rows
```

---

# Practice MCQs

## Java MCQs

### Q1
Which access modifier gives minimum visibility?

A. public  
B. protected  
C. private  
D. default  

**Answer: C. private**

### Q2
Which access modifier gives maximum visibility?

A. private  
B. default  
C. protected  
D. public  

**Answer: D. public**

### Q3
Which of the following is accessible only inside the same class?

A. public  
B. protected  
C. private  
D. default  

**Answer: C. private**

### Q4
Inheritance represents which relationship?

A. HAS-A  
B. USES-A  
C. IS-A  
D. PART-OF  

**Answer: C. IS-A**

### Q5
Which inheritance is not supported through classes in Java?

A. single inheritance  
B. multilevel inheritance  
C. hierarchical inheritance  
D. multiple inheritance  

**Answer: D. multiple inheritance**

### Q6
What will be the output?

```java
class Parent {
    Parent() {
        System.out.println("Parent");
    }
}

class Child extends Parent {
    Child() {
        System.out.println("Child");
    }
}

public class Main {
    public static void main(String[] args) {
        Child c = new Child();
    }
}
```

**Output:**
```
Parent
Child
```

**Reason:** Parent constructor executes before child constructor.

### Q7
What can a child class directly inherit from a parent?

A. only private members  
B. only constructors  
C. accessible non-private members  
D. nothing  

**Answer: C. accessible non-private members**

## SQL MCQs

Assume the table:

| EmpID | Name | Department | Salary |
|-------|------|------------|--------|
| 1 | Sai | AI | 50000 |
| 2 | Rahul | Java | 70000 |
| 3 | Priya | AI | 60000 |
| 4 | Arjun | Data | 80000 |

### Q1
Which query displays all employee records?

A. `SHOW Employee;`  
B. `SELECT * FROM Employee;`  
C. `GET * FROM Employee;`  
D. `PRINT Employee;`  

**Answer: B**

### Q2
Which query displays only employee names?

A. `SELECT Name FROM Employee;`  
B. `SELECT * Name Employee;`  
C. `SHOW Name Employee;`  
D. `SELECT Employee.Name.All;`  

**Answer: A**

### Q3
Which query returns only employees from AI department?

A. `SELECT * FROM Employee WHERE Department = 'AI';`  
B. `SELECT AI FROM Employee;`  
C. `SELECT * FROM AI;`  
D. `SELECT Department = 'AI';`  

**Answer: A**

### Q4
Which query returns unique department names?

A. `SELECT Department FROM Employee;`  
B. `SELECT UNIQUE Department FROM Employee;`  
C. `SELECT DISTINCT Department FROM Employee;`  
D. `SELECT DIFFERENT Department FROM Employee;`  

**Answer: C**

### Q5
Which query sorts employees by salary from highest to lowest?

A. `SELECT * FROM Employee ORDER BY Salary DESC;`  
B. `SELECT * FROM Employee SORT BY Salary DESC;`  
C. `SELECT * FROM Employee ORDER Salary;`  
D. `SELECT * FROM Employee ORDER BY DESC Salary;`  

**Answer: A**

### Q6
What does this query return?

```sql
SELECT COUNT(*)
FROM Employee;
```

A. number of columns  
B. number of rows  
C. number of salary values only  
D. error  

**Answer: B**

---

# Self-Check Questions

## Java
1. What is the difference between private, default, protected, and public?
2. What is the purpose of access modifiers?
3. What is inheritance in Java?
4. What is the IS-A relationship?
5. What does a child class inherit from a parent class?
6. Why doesn't Java support multiple inheritance of classes?
7. What happens when a child object is created?

## SQL
1. What is the difference between DBMS and RDBMS?
2. What is the purpose of a primary key?
3. What is the purpose of a foreign key?
4. What does `SELECT * FROM Employee;` do?
5. What is the use of `WHERE`?
6. What is the use of `DISTINCT`?
7. What is the difference between ascending and descending order in `ORDER BY`?
8. What does `COUNT(*)` do?

---

# Day 1 Personal Weakness Identified

The biggest issue on Day 1 was careless execution, not complete lack of understanding.

**Main problems:**
- Writing syntax carelessly
- Inconsistent naming
- Not doing a final self-check
- Answering too quickly without verifying whether the query fully matches the question

## Day 1 Correction Strategy

### Java Self-Check Before Submission
- [ ] Is the data type correct?
- [ ] Are variable names consistent?
- [ ] Is the constructor written correctly?
- [ ] Does the method return type match the logic?
- [ ] Are method names correct?
- [ ] Are conditions written properly?

### SQL Self-Check Before Submission
- [ ] Is the table name correct?
- [ ] Is the column name correct?
- [ ] Is SELECT spelled correctly?
- [ ] Are string values written in single quotes?
- [ ] Did I answer exactly what the question asked?

---

# Final Day 1 Summary

### OOPS Covered
- Access modifiers
- Inheritance
- Class member visibility
- IS-A relationship
- Constructor behavior in inheritance

### SQL Covered
- Database / DBMS / RDBMS
- Table / Row / Column
- Primary key / Foreign key
- SELECT
- WHERE
- DISTINCT
- ORDER BY
- COUNT

### Overall Takeaway
Concept understanding is developing well, but execution needs discipline and final checking.

---

**End of Day 1 Content**
