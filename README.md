# DBMS Practical Exam Lab Manual

This repository contains the SQL and MongoDB scripts for the DBMS Practical Examination.

## 📁 Important Resources

* **Practical Documentation & Files:** [Click Here to Access Google Drive](https://drive.google.com/drive/folders/1P03k4vtTejHpL_fPW99jEs_2rfHOv7Vj)

---

## Practical No 01: Database and Table Creation (CollegeDB)

### Code

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

-- Insert Data
INSERT INTO Instructor VALUES (1, 'Dr. Sharma', 'sharma@gmail.com', 'Computer Science'), (2, 'Dr. Patil', 'patil@gmail.com', 'Information Technology');
INSERT INTO Student VALUES (101, 'Rahul', 'rahul@gmail.com', 20, 'Pune'), (102, 'Sneha', 'sneha@gmail.com', 21, 'Mumbai'), (103, 'Amit', 'amit@gmail.com', 19, 'Nashik');
INSERT INTO Course VALUES (201, 'Database Management', 4, 1), (202, 'Data Structures', 3, 2);
INSERT INTO Enrollment VALUES (1, 101, 201, '2026-03-10'), (2, 102, 202, '2026-03-10'), (3, 103, 201, '2026-03-10');

```

### Output

```sql
SELECT * FROM Instructor;
SELECT * FROM Student;
SELECT * FROM Course;
SELECT * FROM Enrollment;

```

---

## Practical No 02: Complex Queries and Indexing

### Code

```sql
CREATE DATABASE IF NOT EXISTS StudentDB;
USE StudentDB;

CREATE TABLE IF NOT EXISTS Students (
    student_id INT PRIMARY KEY,
    name VARCHAR(50) NOT NULL,
    age INT,
    department VARCHAR(50)
);

CREATE TABLE IF NOT EXISTS Courses (
    course_id INT PRIMARY KEY,
    course_name VARCHAR(50) NOT NULL,
    credits INT
);

CREATE TABLE IF NOT EXISTS Enrollment (
    student_id INT,
    course_id INT,
    grade CHAR(1),
    PRIMARY KEY (student_id, course_id),
    FOREIGN KEY (student_id) REFERENCES Students(student_id) ON DELETE CASCADE ON UPDATE CASCADE,
    FOREIGN KEY (course_id) REFERENCES Courses(course_id) ON DELETE CASCADE ON UPDATE CASCADE
);

INSERT INTO Students VALUES (1, 'Alice', 20, 'CS'), (2, 'Bob', 22, 'EE'), (3, 'Charlie', 23, 'ME'), (4, 'David', 23, 'CS');
INSERT INTO Courses VALUES (101, 'DBMS', 4), (102, 'OS', 3), (103, 'Networks', 3), (104, 'AI', 4);
INSERT INTO Enrollment VALUES (1,101,'A'), (1,102,'B'), (2,101,'B'), (3,103,'A'), (4,101,'B'), (4,104,'A');

-- Indexes
CREATE INDEX idx_student_age ON Students(age);
CREATE INDEX idx_course_credits ON Courses(credits);

```

---

## Practical No 03: Views and Explain Plan

### Code

```sql
-- CREATE VIEW
CREATE VIEW StudentCourseView AS
SELECT s.student_id, s.name, s.department, c.course_name, e.grade
FROM Students s
JOIN Enrollment e ON s.student_id = e.student_id
JOIN Courses c ON e.course_id = c.course_id;

-- CREATE INDEXES
CREATE INDEX idx_student_name ON Students(name);

```

### Output

```sql
-- DISPLAY VIEW
SELECT * FROM StudentCourseView;

-- EXPLAIN QUERIES
EXPLAIN SELECT * FROM Students WHERE name = 'Alice';

```

---

## Practical No 07: NoSQL Operations (MongoDB)

### Code

```javascript
// Create / Select Database
use StudentDB;

// Insert Students
db.students.insertMany([
  { studentId: 1, name: 'Alice', age: 20, department: 'CS' },
  { studentId: 2, name: 'Bob', age: 22, department: 'EE' },
  { studentId: 3, name: 'Charlie', age: 23, department: 'ME' },
  { studentId: 4, name: 'David', age: 23, department: 'CS' }
]);

// Update Student
db.students.updateOne({ name: 'Alice' }, { $set: { age: 21 } });

```

---

## Practical No 09: Database Backup and Restore

### Backup (Terminal Commands)

```bash
# Backup a Single Database
mysqldump -u root -p CollegeDB > CollegeDB_backup.sql

# Restore from Backup File
mysql -u root -p CollegeDB < CollegeDB_backup.sql

```

### Output Verification

```sql
-- Show Tables
SHOW TABLES;
-- Verify Data
SELECT * FROM Student;

```
