# DBMS Practical Exam Lab Manual

This repository contains the SQL and MongoDB scripts for the DBMS Practical Examination.

## 📁 Important Resources

* **Practical Documentation & Files:** [Click Here to Access Google Drive](https://drive.google.com/drive/folders/1P03k4vtTejHpL_fPW99jEs_2rfHOv7Vj)

---
---

## Practical No 01: Database and Table Creation (CollegeDB)

### 💻 Code (Input)

```sql
-- Create Database
CREATE DATABASE CollegeDB;
USE CollegeDB;

-- Create Instructor Table
CREATE TABLE Instructor (
    InstructorID INT PRIMARY KEY,
    Name VARCHAR(50),
    Email VARCHAR(50),
    Department VARCHAR(50)
);

-- Create Student Table
CREATE TABLE Student (
    StudentID INT PRIMARY KEY,
    Name VARCHAR(50),
    Email VARCHAR(50),
    Age INT,
    Address VARCHAR(100)
);

-- Create Course Table
CREATE TABLE Course (
    CourseID INT PRIMARY KEY,
    CourseName VARCHAR(50),
    Credits INT,
    InstructorID INT,
    FOREIGN KEY (InstructorID) REFERENCES Instructor(InstructorID)
);

-- Create Enrollment Table
CREATE TABLE Enrollment (
    EnrollmentID INT PRIMARY KEY,
    StudentID INT,
    CourseID INT,
    EnrollmentDate DATE,
    FOREIGN KEY (StudentID) REFERENCES Student(StudentID),
    FOREIGN KEY (CourseID) REFERENCES Course(CourseID)
);

-- Insert Sample Data
INSERT INTO Instructor VALUES
    (1, 'Dr. Sharma', 'sharma@gmail.com', 'Computer Science'),
    (2, 'Dr. Patil', 'patil@gmail.com', 'Information Technology');

INSERT INTO Student VALUES
    (101, 'Rahul', 'rahul@gmail.com', 20, 'Pune'),
    (102, 'Sneha', 'sneha@gmail.com', 21, 'Mumbai'),
    (103, 'Amit', 'amit@gmail.com', 19, 'Nashik');

INSERT INTO Course VALUES
    (201, 'Database Management', 4, 1),
    (202, 'Data Structures', 3, 2);

INSERT INTO Enrollment VALUES
    (1, 101, 201, '2026-03-10'),
    (2, 102, 202, '2026-03-10'),
    (3, 103, 201, '2026-03-10');

```

### 📄 Output (Verification)

```sql
-- Display all data to verify setup
SELECT * FROM Instructor;
SELECT * FROM Student;
SELECT * FROM Course;
SELECT * FROM Enrollment;

```
## Practical No 02: Complex Queries and Indexing

### 💻 Code (Input)

```sql
-- Create Database
CREATE DATABASE IF NOT EXISTS StudentDB;
USE StudentDB;

-- Create Students Table
CREATE TABLE IF NOT EXISTS Students (
    student_id INT PRIMARY KEY,
    name VARCHAR(50) NOT NULL,
    age INT,
    department VARCHAR(50)
);

-- Create Courses Table
CREATE TABLE IF NOT EXISTS Courses (
    course_id INT PRIMARY KEY,
    course_name VARCHAR(50) NOT NULL,
    credits INT
);

-- Create Enrollment Table
CREATE TABLE IF NOT EXISTS Enrollment (
    student_id INT,
    course_id INT,
    grade CHAR(1),
    PRIMARY KEY (student_id, course_id),
    FOREIGN KEY (student_id) REFERENCES Students(student_id)
        ON DELETE CASCADE ON UPDATE CASCADE,
    FOREIGN KEY (course_id) REFERENCES Courses(course_id)
        ON DELETE CASCADE ON UPDATE CASCADE
);

-- Insert Data
INSERT INTO Students VALUES
(1, 'Alice', 20, 'CS'),
(2, 'Bob', 22, 'EE'),
(3, 'Charlie', 23, 'ME'),
(4, 'David', 23, 'CS');

INSERT INTO Courses VALUES
(101, 'DBMS', 4),
(102, 'OS', 3),
(103, 'Networks', 3),
(104, 'AI', 4);

INSERT INTO Enrollment VALUES
(1, 101, 'A'),
(1, 102, 'B'),
(2, 101, 'B'),
(3, 103, 'A'),
(4, 101, 'B'),
(4, 104, 'A');

-- Create Indexes
CREATE INDEX idx_student_age ON Students(age);
CREATE INDEX idx_course_credits ON Courses(credits);
CREATE INDEX idx_enrollment ON Enrollment(student_id, course_id);

```

### 📄 Output (Verification Queries)

```sql
-- 1. Students older than 21
SELECT * FROM Students WHERE age > 21;

-- 2. Students from CS department
SELECT * FROM Students WHERE department = 'CS';

-- 3. Average age per department
SELECT department, AVG(age) AS avg_age FROM Students GROUP BY department;

-- 4. Count students per course
SELECT c.course_name, COUNT(e.student_id) AS student_count
FROM Courses c
JOIN Enrollment e ON c.course_id = e.course_id
GROUP BY c.course_name;

-- 5. Explain Query to show Indexing effect
EXPLAIN SELECT * FROM Students WHERE age > 21;

```

## Practical No 03: Views and Indexing

### 💻 Code (Input)

```sql
-- Create Database
CREATE DATABASE IF NOT EXISTS StudentDB;
USE StudentDB;

-- Create Students Table
CREATE TABLE IF NOT EXISTS Students (
	student_id INT PRIMARY KEY,
	name VARCHAR(50),
	age INT,
	department VARCHAR(50)
);

-- Create Courses Table
CREATE TABLE IF NOT EXISTS Courses (
	course_id INT PRIMARY KEY,
	course_name VARCHAR(50),
	credits INT
);

-- Create Enrollment Table
CREATE TABLE IF NOT EXISTS Enrollment (
	enrollment_id INT PRIMARY KEY,
	student_id INT,
	course_id INT,
	grade CHAR(2),
	FOREIGN KEY (student_id) REFERENCES Students(student_id),
	FOREIGN KEY (course_id) REFERENCES Courses(course_id)
);

-- Insert Sample Data
INSERT INTO Students VALUES (1, 'Alice', 20, 'CS'), (2, 'Bob', 21, 'EE'), (3, 'Charlie', 22, 'ME');
INSERT INTO Courses VALUES (101, 'DBMS', 4), (102, 'OS', 3), (103, 'Networks', 3);
INSERT INTO Enrollment VALUES (1, 1, 101, 'A'), (2, 1, 102, 'B'), (3, 2, 101, 'B'), (4, 3, 103, 'A');

-- CREATE VIEW
CREATE VIEW StudentCourseView AS
SELECT
	s.student_id,
	s.name,
	s.department,
	c.course_name,
	e.grade
FROM Students s
JOIN Enrollment e ON s.student_id = e.student_id
JOIN Courses c ON e.course_id = c.course_id;

-- CREATE INDEXES
CREATE INDEX idx_student_name ON Students(name);
CREATE INDEX idx_course_name ON Courses(course_name);

```

### 📄 Output (Verification Queries)

```sql
-- 1. Display View Data
SELECT * FROM StudentCourseView;

-- 2. Update record via View
UPDATE StudentCourseView
SET grade = 'A'
WHERE name = 'Alice' AND course_name = 'DBMS';

-- 3. Delete record via View
DELETE FROM StudentCourseView
WHERE name = 'Charlie' AND course_name = 'Networks';

-- 4. Final Display of View
SELECT * FROM StudentCourseView;

-- 5. Explain Query (Indexing check)
EXPLAIN SELECT * FROM Students WHERE name = 'Alice';

```

## Practical No 04: SQL Joins

### 💻 Code (Input)

```sql
-- Create Database
CREATE DATABASE IF NOT EXISTS StudentDB;
USE StudentDB;

-- Table Cleanup
SET FOREIGN_KEY_CHECKS = 0;
DROP TABLE IF EXISTS Orders;
DROP TABLE IF EXISTS Customers;
SET FOREIGN_KEY_CHECKS = 1;

-- Create Customers Table
CREATE TABLE Customers (
	CustomerID INT PRIMARY KEY,
	CustomerName VARCHAR(50),
	ContactNumber VARCHAR(15)
);

-- Create Orders Table
CREATE TABLE Orders (
	OrderID INT PRIMARY KEY,
	CustomerID INT,
	OrderDate DATE,
	Amount DECIMAL(10,2),
	FOREIGN KEY (CustomerID) REFERENCES Customers(CustomerID)
);

-- Insert Sample Data
INSERT INTO Customers VALUES
(1, 'Alice', '1234567890'),
(2, 'Bob', '2345678901'),
(3, 'Charlie', '3456789012'),
(4, 'David', '4567890123');

INSERT INTO Orders VALUES
(101, 1, '2026-03-15', 250.00),
(102, 2, '2026-03-16', 450.00),
(103, 2, '2026-03-17', 150.00),
(104, 3, '2026-03-18', 300.00);

```

### 📄 Output (Verification Queries)

```sql
-- 1. INNER JOIN (Matching records only)
SELECT c.CustomerID, c.CustomerName, o.OrderID, o.Amount
FROM Customers c
INNER JOIN Orders o ON c.CustomerID = o.CustomerID;

-- 2. LEFT JOIN (All customers, even without orders)
SELECT c.CustomerID, c.CustomerName, o.OrderID, o.Amount
FROM Customers c
LEFT JOIN Orders o ON c.CustomerID = o.CustomerID;

-- 3. RIGHT JOIN (All orders, even if customer data is missing)
SELECT c.CustomerID, c.CustomerName, o.OrderID, o.Amount
FROM Customers c
RIGHT JOIN Orders o ON c.CustomerID = o.CustomerID;

-- 4. FULL OUTER JOIN (MySQL workaround using UNION)
SELECT c.CustomerID, c.CustomerName, o.OrderID, o.Amount
FROM Customers c
LEFT JOIN Orders o ON c.CustomerID = o.CustomerID
UNION
SELECT c.CustomerID, c.CustomerName, o.OrderID, o.Amount
FROM Customers c
RIGHT JOIN Orders o ON c.CustomerID = o.CustomerID;

```
## Practical No 07: NoSQL Operations (MongoDB)

### 💻 Code (Input)

```javascript
// Create / Select Database
use StudentDB;

// Insert Students Data
db.students.insertMany([
  { studentId: 1, name: 'Alice', age: 20, department: 'CS' },
  { studentId: 2, name: 'Bob', age: 22, department: 'EE' },
  { studentId: 3, name: 'Charlie', age: 23, department: 'ME' },
  { studentId: 4, name: 'David', age: 23, department: 'CS' }
]);

// Update a Student's age
db.students.updateOne(
  { name: 'Alice' },
  { $set: { age: 21 } }
);

// Delete a Student
db.students.deleteOne({
  name: 'David'
});

// Create Indexes for performance
db.students.createIndex({ name: 1 });
db.students.createIndex({ age: 1 });

```

### 📄 Output (Verification Queries)

```javascript
// 1. Find All Students
db.students.find().pretty();

// 2. Find Students with filter (Age > 21)
db.students.find({ age: { $gt: 21 } });

// 3. Aggregation – Average Age by Department
db.students.aggregate([
  {
    $group: {
      _id: '$department',
      avgAge: { $avg: '$age' }
    }
  }
]);

// 4. Aggregation – Count per Department sorted by count
db.students.aggregate([
  { $group: { _id: '$department', count: { $sum: 1 } } },
  { $sort: { count: -1 } }
]);

// 5. Verify Index Usage
db.students.find({ name: 'Alice' }).explain('executionStats');

```

## Practical No 09: Database Backup and Restore

### 💻 Code (Input - Terminal Commands)

```bash
# ======= BACKUP OPERATIONS =======

# 1. Backup a Single Database to a .sql file
mysqldump -u root -p CollegeDB > CollegeDB_backup.sql

# 2. Backup Only the Table Structure (No Data)
mysqldump -u root -p --no-data CollegeDB > CollegeDB_schema.sql

# 3. Backup a Specific Table (Student Table)
mysqldump -u root -p CollegeDB Student > student_backup.sql


# ======= RESTORE OPERATIONS =======

# 1. Create the Database if it doesn't exist
mysql -u root -p -e "CREATE DATABASE IF NOT EXISTS CollegeDB;"

# 2. Restore the Database from the backup file
mysql -u root -p CollegeDB < CollegeDB_backup.sql

```

### 📄 Output (Verification Queries)

```sql
-- Run these inside the MySQL shell to verify the restore
USE CollegeDB;

-- 1. Check if tables exist
SHOW TABLES;

-- 2. Verify data integrity in Student table
SELECT * FROM Student;

-- 3. Check Instructor data
SELECT * FROM Instructor;

```
