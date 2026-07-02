#  Day 1 - Access Modifiers and Inheritance and SQL

## Why this topic matters

Access modifiers and inheritance are foundational OOP concepts in Java.  
They are important not just for theory questions, but also for:

- output-based Java MCQs
- error spotting questions
- understanding encapsulation
- understanding how parent-child classes work
- learning advanced topics like polymorphism, overriding, Spring Boot class design, and framework structure

---

# 1. Access Modifiers

## Definition

Access modifiers control the visibility of class members.

Class members include:
- variables / fields
- methods
- constructors
- classes (in some cases)

In simple words, access modifiers decide:

> **Who can access what?**

---

## Why access modifiers are needed

Imagine a banking application:

```java
class BankAccount {
    String accountHolder;
    double balance;
}

If balance is openly accessible, any class can do:

account.balance = -5000;

or

account.balance = 999999999;

This is dangerous.

So Java provides access modifiers to:

protect data
restrict visibility
control how objects are used
support encapsulation
2. Types of Access Modifiers in Java

Java has 4 access levels:

private
default / package-private (no keyword)
protected
public
3. private
Definition

A private member can be accessed only inside the same class.

Example:

class BankAccount {
    private double balance;
}

Inside the same class, it is accessible:

class BankAccount {
    private double balance;

    void showBalance() {
        System.out.println(balance);
    }
}

Outside the class, it is not accessible:

BankAccount b = new BankAccount();
b.balance = 1000;   // Error
Why private is important

private is the backbone of encapsulation.

Important internal data should not be modified directly from outside the class.

Instead of doing this:

b.balance = -5000;

we should provide controlled methods like:

deposit()
withdraw()
getBalance()

This allows validation and protection.

Real-world use of private
Banking app

Fields like:

balance
ATM pin
transaction history

should not be directly modified by outside classes.

E-commerce app

Fields like:

stock quantity
payment status
internal discount logic

should be protected.

Student portal / HR systems

Sensitive data like:

marks
salary
attendance records

should not be openly accessible.

4. Default Access Modifier (Package-Private)

If no modifier is written, Java gives default access.

Example:

class Student {
    int age;
}

Here age has default access.

Meaning

A default member is accessible:

inside the same class
inside the same package

It is not accessible outside the package.

Why default access exists

Sometimes a member does not need to be public for the whole world, but it can still be shared among closely related classes in the same package.

Think of a package as a small team or module.

Classes inside that module can use default members.

5. protected
Definition

A protected member is accessible:

inside the same class
inside the same package
in child classes (subclasses)

Example:

class Employee {
    protected double salary;
}

If another class extends Employee, it can access salary.

Why protected exists

Sometimes a parent class wants to share certain data or methods with its child classes, but not expose them to everyone.

For example:

Parent class: Vehicle

Child classes:

Car
Bike
Truck

A child class may need access to some parent-level fields or methods, but unrelated external classes should not.

That is where protected becomes useful.

6. public
Definition

A public member is accessible from anywhere.

Example:

public class Student {
    public String name;
}

If a class is visible/imported properly, any class can access a public member.

Real-world use of public

Methods that are part of the official API of a class are often public.

Examples:

deposit()
withdraw()
getBalance()
startEngine()
displayProfile()
7. Access Modifier Visibility Table

This table is important. Memorize it.

Modifier	Same Class	Same Package	Child Class	Everywhere
private	Yes	No	No	No
default	Yes	Yes	No*	No
protected	Yes	Yes	Yes	No
public	Yes	Yes	Yes	Yes

* Default does not give special access to child classes outside the package.

8. Proper Use of Access Modifiers
Bad design
class BankAccount {
    public double balance;
}

Problem:

any class can change balance directly
invalid values can be assigned
no validation is possible
Better design
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

Advantages:

data is protected
operations are controlled
validation is possible
object state remains safe
9. Inheritance
Definition

Inheritance is the mechanism by which one class acquires the properties and behaviors of another class.

In Java, inheritance is implemented using the keyword:

extends
Example
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

Now Car can use:

brand
start()
playMusic()
10. Parent and Child Class Terminology

If:

class Vehicle { }
class Car extends Vehicle { }

then:

Parent class
superclass
base class
Child class
subclass
derived class
11. Why inheritance exists

Inheritance exists mainly to avoid duplication and model real-world relationships.

Suppose we have:

Car
Bike
Truck

All of them have:

brand
speed
start()
stop()

Without inheritance, we would write the same code repeatedly in multiple classes.

Inheritance allows us to keep common properties in one parent class and reuse them in child classes.

12. Core idea of inheritance

If a child class extends a parent class, the child can use the parent’s accessible members.

Example:

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

Usage:

Dog d = new Dog();
d.name = "Bruno";
d.eat();
d.bark();

Output:

Animal is eating
Dog is barking
13. IS-A Relationship

Inheritance should be used only when there is a true IS-A relationship.

Examples:

Car is a Vehicle
Dog is an Animal
SavingsAccount is a BankAccount
Manager is an Employee

Bad examples:

Engine is a Car ❌
Address is a Student ❌
Customer is a Bank ❌

If there is no real IS-A relationship, inheritance is usually the wrong choice.

14. Real-world applications of inheritance
Banking system

Parent:

BankAccount

Child classes:

SavingsAccount
CurrentAccount
SalaryAccount

Common data:

account number
account holder
balance
deposit()
withdraw()

Specific child data:

interest rate for savings account
overdraft limit for current account
E-commerce system

Parent:

Product

Child classes:

Book
Electronics
Clothing

Common:

productId
name
price

Specific:

author for books
warranty for electronics
size for clothing
HR software

Parent:

Employee

Child classes:

Developer
Manager
HR

Common:

employeeId
name
salary

Specific:

programmingLanguage for Developer
teamSize for Manager
15. What gets inherited?

A child class can inherit:

non-private variables
non-private methods

A child class cannot directly access private members of the parent.

Example:

class Parent {
    private int x = 10;
}

class Child extends Parent {
}

Child does not get direct access to x.

16. Inheritance and Access Modifiers Together

Example:

class Employee {
    private int id;
    protected double salary;
    public String name;
}

In a child class:

id cannot be directly accessed
salary can be accessed
name can be accessed

This is why protected is especially relevant in inheritance-based code.

17. Types of Inheritance in Java
1. Single Inheritance

One child inherits from one parent.

class A { }
class B extends A { }
2. Multilevel Inheritance

A chain of inheritance.

class Animal { }
class Dog extends Animal { }
class Puppy extends Dog { }

Here Puppy indirectly gets features from Animal.

3. Hierarchical Inheritance

Multiple child classes inherit from one parent.

class Vehicle { }
class Car extends Vehicle { }
class Bike extends Vehicle { }
class Truck extends Vehicle { }

This is common in real software design.

4. Multiple Inheritance of Classes

Java does not support multiple inheritance using classes.

This is invalid:

class C extends A, B { }

Reason:

ambiguity problems
diamond problem

Java handles multiple-type behavior using interfaces, which will be studied later.

18. Constructor Behavior in Inheritance

When a child object is created, the parent constructor executes first.

Example:

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
Child c = new Child();

Output:

Parent constructor
Child constructor
Why does this happen?

A child object contains the parent portion also.
So when Java creates a child object, it first initializes the parent part and then the child part.

This becomes very important when studying the super keyword.

19. Method Overriding - Preview

Sometimes a child class wants to provide its own version of a parent method.

Example:

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

This is called method overriding.

It will be studied properly in the next topic along with polymorphism.

20. When NOT to use inheritance

Do not use inheritance just because two classes share some fields.

Use inheritance only when:

there is a genuine IS-A relationship
child is a specialized version of parent
common behavior is meaningful to share

Bad use cases:

Engine and Car
Address and Student
Keyboard and Computer

These are better represented using composition / has-a relationship.

21. Key Differences: Encapsulation vs Inheritance
Encapsulation

Encapsulation means:

hide data
expose controlled methods
protect internal state

Example:

private balance
public deposit()
Inheritance

Inheritance means:

one class reuses and extends another class
common behavior is shared through parent-child relationship

Example:

Car extends Vehicle
22. Summary / Revision Notes
Access Modifiers

Access modifiers control visibility of class members.

private
accessible only inside the same class
used for data hiding
default
accessible within the same package
protected
accessible within the same package
accessible in child classes
public
accessible everywhere
Inheritance

Inheritance allows one class to acquire the properties and behaviors of another class.

Java keyword:

extends
Benefits
code reuse
avoids duplication
creates logical class hierarchy
models real-world relationships
Important rule

Use inheritance only for a true IS-A relationship.

Types in Java
single inheritance
multilevel inheritance
hierarchical inheritance
Not supported with classes
multiple inheritance of classes
Constructor rule

When child object is created, parent constructor runs first.

23. Quick Memory Sheet
private   -> only same class
default   -> same package
protected -> same package + child class
public    -> everywhere

Inheritance = child class extends parent class
Use inheritance only for IS-A relationship
Child gets accessible members of parent
Private members are not directly accessible in child
Parent constructor runs before child constructor
24. Practice MCQs
Q1

Which access modifier gives minimum visibility?

A. public
B. protected
C. private
D. default

Answer: C. private

Q2

Which access modifier gives maximum visibility?

A. private
B. default
C. protected
D. public

Answer: D. public

Q3

Which of the following is accessible only inside the same class?

A. public
B. protected
C. private
D. default

Answer: C. private

Q4

Inheritance represents which relationship?

A. HAS-A
B. USES-A
C. IS-A
D. PART-OF

Answer: C. IS-A

Q5

Which inheritance is not supported through classes in Java?

A. single inheritance
B. multilevel inheritance
C. hierarchical inheritance
D. multiple inheritance

Answer: D. multiple inheritance

Q6

What will be the output?

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

Output:

Parent
Child

Reason: parent constructor executes before child constructor.

Q7

What can a child class directly inherit from a parent?

A. only private members
B. only constructors
C. accessible non-private members
D. nothing

Answer: C. accessible non-private members


---

# FILE 2 — `SQL/Day1_SQL_Basics.md`

Paste this as it is.

---

```md
# SQL Day 1 - SQL Basics, DBMS, RDBMS, SELECT, WHERE, DISTINCT, ORDER BY, COUNT

## Why this topic matters

SQL is one of the most important parts of Infosys training and pretests.  
Even if the SQL questions become moderate or advanced later, the advanced part is built on these basic concepts.

If these basics are weak, then:
- JOINs become confusing
- GROUP BY and HAVING become messy
- subqueries become painful
- large-table questions become overwhelming

So this first layer must be clean.

---

# 1. What is data?

Data means raw facts or information.

Examples:
- employee name
- student roll number
- department name
- salary
- phone number
- order id

If a company like Infosys has 50,000 employees, all their names, ids, departments, salaries, project details etc. are data.

---

# 2. What is a database?

A database is an organized collection of data.

Example: an employee database may store records like this:

| EmpID | Name  | Department | Salary |
|------|-------|------------|--------|
| 101  | Sai   | AI         | 50000  |
| 102  | Rahul | Java       | 70000  |
| 103  | Priya | Data       | 65000  |

Instead of storing this data randomly in notebooks or multiple disconnected files, a database stores it in an organized way so that it can be searched, updated, and managed efficiently.

---

# 3. What is DBMS?

DBMS = **Database Management System**

A DBMS is software used to create, manage, store, retrieve, and manipulate data in a database.

Examples of DBMS / database systems:
- MySQL
- Oracle
- PostgreSQL
- SQL Server

A DBMS allows us to:
- create tables
- insert records
- update records
- delete records
- retrieve records
- manage large amounts of data safely

---

# 4. What is RDBMS?

RDBMS = **Relational Database Management System**

An RDBMS stores data in the form of **tables** and allows relationships between tables.

Example:

## Employee table

| EmpID | Name  | DeptID |
|------|-------|--------|
| 1    | Sai   | 10     |
| 2    | Rahul | 20     |

## Department table

| DeptID | DeptName |
|--------|----------|
| 10     | AI       |
| 20     | Java     |

Here the `DeptID` in Employee table is related to the `DeptID` in Department table.  
That is why it is called a **relational** database system.

---

# 5. Table, Row, and Column

## Table
A table is a collection of related data arranged in rows and columns.

Example: `Employee` table.

---

## Row
A row represents one complete record.

Example:

| 1 | Sai | AI | 50000 |

This entire record is one row.

---

## Column
A column represents one attribute / field of the data.

Examples:
- EmpID
- Name
- Department
- Salary

---

# 6. Primary Key

A primary key is a column (or a set of columns) that uniquely identifies each row in a table.

Example:

| EmpID | Name  |
|------|-------|
| 101  | Sai   |
| 102  | Rahul |

Here `EmpID` can be the primary key because it uniquely identifies each employee.

## Properties of primary key
- unique
- not null
- one primary key per table (though it can be composite)

---

# 7. Foreign Key

A foreign key is a column in one table that refers to the primary key of another table.

Example:

## Department table

| DeptID | DeptName |
|--------|----------|
| 10     | AI       |
| 20     | Java     |

## Employee table

| EmpID | Name  | DeptID |
|------|-------|--------|
| 1    | Sai   | 10     |
| 2    | Rahul | 20     |

Here `Employee.DeptID` is a foreign key referencing `Department.DeptID`.

---

## Why foreign keys are important

Foreign keys create relationships between tables and help maintain consistency.

For example:
- an employee’s department should ideally exist in the Department table
- orders should refer to valid customers
- students should refer to valid courses or branches

---

# 8. What is SQL?

SQL = **Structured Query Language**

SQL is the language used to communicate with relational databases.

Using SQL, we can:
- retrieve data
- filter data
- sort data
- count records
- insert new records
- update records
- delete records

For now, the focus is on retrieving and filtering data.

---

# 9. SELECT

`SELECT` is used to retrieve data from a table.

Think:

> **SELECT = show me data**

---

## Syntax

```sql
SELECT column_name
FROM table_name;

or

SELECT *
FROM table_name;
Example table: Employee
EmpID	Name	Department	Salary
1	Sai	AI	50000
2	Rahul	Java	70000
3	Priya	AI	60000
4	Arjun	Data	80000
Example 1 - Show all columns
SELECT * FROM Employee;

Meaning:

SELECT * → select all columns
FROM Employee → from Employee table
Example 2 - Show selected columns
SELECT Name, Salary
FROM Employee;

This displays only the Name and Salary columns.

10. WHERE

WHERE is used to filter rows based on a condition.

Think:

WHERE = keep only the rows that satisfy the condition

Example 1
SELECT *
FROM Employee
WHERE Department = 'AI';

This returns only the employees whose department is AI.

Example 2
SELECT Name, Salary
FROM Employee
WHERE Salary > 55000;

This returns the name and salary of employees whose salary is greater than 55,000.

11. DISTINCT

DISTINCT is used to remove duplicate values from the result.

Suppose departments in Employee table are:

AI
Java
AI
Data

If we write:

SELECT Department
FROM Employee;

we will get repeated values.

If we want only unique department names:

SELECT DISTINCT Department
FROM Employee;

Output:

AI
Java
Data
12. ORDER BY

ORDER BY is used to sort the result.

Ascending order
SELECT *
FROM Employee
ORDER BY Salary;

or explicitly:

SELECT *
FROM Employee
ORDER BY Salary ASC;

Ascending is the default order.

Descending order
SELECT *
FROM Employee
ORDER BY Salary DESC;

This shows highest salary first.

13. COUNT()

COUNT() is used to count rows / records.

Count all rows
SELECT COUNT(*)
FROM Employee;

This gives the total number of employee records.

Count with a condition
SELECT COUNT(*)
FROM Employee
WHERE Department = 'AI';

This gives the number of employees in the AI department.

14. How to read a SQL query in English

This is very important for big-table questions.

Example:

SELECT Name, Salary
FROM Employee
WHERE Salary > 50000
ORDER BY Salary DESC;

Read it step by step:

Start with the Employee table
Keep only rows where salary > 50000
From those rows, show only Name and Salary
Sort the result by salary in descending order

If you learn to translate queries into plain English, SQL becomes much easier.

15. Difference Between DBMS and RDBMS
Feature	DBMS	RDBMS
Full form	Database Management System	Relational Database Management System
Data storage	Can store data in different forms	Stores data in tables
Relationships	May or may not support relations	Supports relations between tables
Example	general DB systems	MySQL, Oracle, PostgreSQL, SQL Server

In interviews and training contexts, when people say “SQL database,” they usually mean an RDBMS.

16. Important Syntax Notes
1. String values should be in single quotes

Correct:

WHERE Department = 'AI'

Avoid writing:

WHERE Department = "AI"

unless a specific DBMS allows it and you know exactly what you are doing.

For learning and assessments, use single quotes for string values.

2. Table names and column names must be consistent

If the table is Employee, don’t write:

Employees
EMPLOYEES
EmployeeData

unless the actual table name is that.

Similarly:

Department is not the same as Departments
Salary is not the same as Salaries
3. SQL keyword spelling matters

Examples of common mistakes:

SELEF instead of SELECT
SELLET instead of SELECT

These are silly mistakes but they can still cost marks or make queries invalid.

17. Summary / Revision Notes
Database

An organized collection of data.

DBMS

Software used to manage databases.

RDBMS

A DBMS that stores data in related tables.

Table

A collection of related records.

Row

One complete record.

Column

One field / attribute.

Primary Key

A column that uniquely identifies each row.

Foreign Key

A column that refers to the primary key of another table.

SQL Clauses Covered
SELECT

Used to retrieve data.

SELECT * FROM Employee;
SELECT Name, Salary FROM Employee;
WHERE

Used to filter rows.

SELECT * FROM Employee
WHERE Department = 'AI';
DISTINCT

Used to remove duplicates.

SELECT DISTINCT Department
FROM Employee;
ORDER BY

Used to sort results.

SELECT * FROM Employee ORDER BY Salary;
SELECT * FROM Employee ORDER BY Salary DESC;
COUNT()

Used to count rows.

SELECT COUNT(*) FROM Employee;
18. Quick Memory Sheet
Database -> organized collection of data
DBMS -> software to manage databases
RDBMS -> database system with related tables

SELECT   -> retrieve data
WHERE    -> filter rows
DISTINCT -> remove duplicates
ORDER BY -> sort rows
COUNT()  -> count rows
19. Practice MCQs

Assume the table:

EmpID	Name	Department	Salary
1	Sai	AI	50000
2	Rahul	Java	70000
3	Priya	AI	60000
4	Arjun	Data	80000
Q1

Which query displays all employee records?

A.

SHOW Employee;

B.

SELECT * FROM Employee;

C.

GET * FROM Employee;

D.

PRINT Employee;

Answer: B

Q2

Which query displays only employee names?

A.

SELECT Name FROM Employee;

B.

SELECT * Name Employee;

C.

SHOW Name Employee;

D.

SELECT Employee.Name.All;

Answer: A

Q3

Which query returns only employees from AI department?

A.

SELECT * FROM Employee WHERE Department = 'AI';

B.

SELECT AI FROM Employee;

C.

SELECT * FROM AI;

D.

SELECT Department = 'AI';

Answer: A

Q4

Which query returns unique department names?

A.

SELECT Department FROM Employee;

B.

SELECT UNIQUE Department FROM Employee;

C.

SELECT DISTINCT Department FROM Employee;

D.

SELECT DIFFERENT Department FROM Employee;

Answer: C

Q5

Which query sorts employees by salary from highest to lowest?

A.

SELECT * FROM Employee ORDER BY Salary DESC;

B.

SELECT * FROM Employee SORT BY Salary DESC;

C.

SELECT * FROM Employee ORDER Salary;

D.

SELECT * FROM Employee ORDER BY DESC Salary;

Answer: A

Q6

What does this query return?

SELECT COUNT(*)
FROM Employee;

A. number of columns
B. number of rows
C. number of salary values only
D. error

Answer: B

20. Self-check Questions
What is the difference between DBMS and RDBMS?
What is the purpose of a primary key?
What is the purpose of a foreign key?
What does SELECT * FROM Employee; do?
What is the use of WHERE?
What is the use of DISTINCT?
What is the difference between ascending and descending order in ORDER BY?
What does COUNT(*) do?

---

# FILE 3 — `Practice/Day1_Assignments_and_Answers.md`

Paste this as it is.

---

```md
# Day 1 Practice - Assignments, Correct Answers, Mistake Log

This file contains:
1. Day 1 assignment questions
2. Correct answers
3. Mistakes identified in the first attempt
4. Lessons learned

---

# Part A - OOPS Assignment

## Question

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

---

## Correct Answer

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
Concept Check for this Assignment

This assignment tests:

class creation
private fields
constructor creation
this keyword
encapsulation
method writing
basic validation logic
Part B - SQL Assignment

Assume the table:

EmpID	Name	Department	Salary
1	Sai	AI	50000
2	Rahul	Java	70000
3	Priya	AI	60000
4	Arjun	Data	80000
Questions
Show all employees
Show only Name and Salary
Show only AI department employees
Show employees with salary > 60000
Show unique departments
Sort salary ascending
Sort salary descending
Count total employees
Correct Answers
1. Show all employees
SELECT * FROM Employee;
2. Show only Name and Salary
SELECT Name, Salary
FROM Employee;
3. Show only AI department employees
SELECT *
FROM Employee
WHERE Department = 'AI';
4. Show employees with salary > 60000
SELECT *
FROM Employee
WHERE Salary > 60000;
5. Show unique departments
SELECT DISTINCT Department
FROM Employee;
6. Sort salary ascending
SELECT Salary
FROM Employee
ORDER BY Salary;

Alternative acceptable answer:

SELECT *
FROM Employee
ORDER BY Salary;
7. Sort salary descending
SELECT Salary
FROM Employee
ORDER BY Salary DESC;

Alternative acceptable answer:

SELECT *
FROM Employee
ORDER BY Salary DESC;
8. Count total employees
SELECT COUNT(*)
FROM Employee;
Mistake Log - First Attempt
Java mistakes made
1. Wrong field declaration

Incorrect:

private String double balance;

Correct:

private double balance;

Reason:
A variable can have only one data type. balance must be numeric, so double is the correct type.

2. Wrong return type for deposit()

Incorrect idea:

public double deposit(double amount)

Correct:

public void deposit(double amount)

Reason:
deposit() only performs an action. It does not return a value in this assignment.

3. Wrong return type for withdraw()

Incorrect idea:

public double withdraw(double amount)

Correct:

public void withdraw(double amount)

Reason:
withdraw() only updates the balance. It does not return a value here.

4. Typo in getter name

Incorrect:

gteBalance()

Correct:

getBalance()
5. Naming inconsistency risk

Use ownerName consistently instead of mixing forms like:

OwnerName
ownerName
SQL mistakes made
1. Table name inconsistency

Incorrect:

SELECT * FROM Employees;

Correct:

SELECT * FROM Employee;

Reason:
Table names must match exactly.

2. Column name inconsistency

Incorrect:

SELECT DISTINCT Departments FROM Employee;

Correct:

SELECT DISTINCT Department FROM Employee;

Reason:
The actual column name is Department, not Departments.

3. SQL keyword typo

Incorrect:

SELEF
SELLET

Correct:

SELECT
4. Narrow answer to a broader question

Question:
Show only AI department employees

Incorrect narrow answer:

SELECT Salary
FROM Employee
WHERE Department = 'AI';

Better answer:

SELECT *
FROM Employee
WHERE Department = 'AI';

Reason:
The question asked for employees from AI department, not only their salaries.

What I Learned from Day 1
OOPS
private is used for data hiding
public methods can be used to access or modify data in a controlled way
constructor is used to initialize object state
this refers to the current object
method return type must match what the method actually does
SQL
SELECT retrieves data
WHERE filters rows
DISTINCT removes duplicates
ORDER BY sorts rows
COUNT(*) counts rows
table names and column names must be written carefully
SQL keyword typos can ruin otherwise correct logic
Day 1 Personal Weakness Identified

The biggest issue on Day 1 was careless execution, not complete lack of understanding.

Main problems:

writing syntax carelessly

inconsistent naming

not doing a final self-check

answering too quickly without verifying whether the query fully matches the question

Day 1 Correction Strategy

Java self-check before submission

Is the data type correct?

Are variable names consistent?

Is the constructor written correctly?

Does the method return type match the logic?

Are method names correct?

Are conditions written properly?

SQL self-check before submission

Is the table name correct?

Is the column name correct?

Is SELECT spelled correctly?

Are string values written in single quotes?

Did I answer exactly what the question asked?

#Final Day 1 Summary

OOPS covered
access modifiers
inheritance
class member visibility
IS-A relationship
constructor behavior in inheritance

SQL covered

database / DBMS / RDBMS
table / row / column
primary key / foreign key
SELECT
WHERE
DISTINCT
ORDER BY
COUNT


Overall takeaway

Concept understanding is developing well, but execution needs discipline and final checking.
