# student_management-database
-- =========================
-- 1. CREATE DATABASE
-- =========================
CREATE DATABASE student_management;
USE student_management;

-- =========================
-- 2. STUDENTS TABLE
-- =========================
CREATE TABLE students (
    student_id INT PRIMARY KEY,
    name VARCHAR(50),
    department VARCHAR(50),
    year INT
);

-- =========================
-- 3. COURSES TABLE
-- =========================
CREATE TABLE courses (
    course_id INT PRIMARY KEY,
    course_name VARCHAR(100),
    faculty VARCHAR(50)
);

-- =========================
-- 4. MARKS TABLE
-- =========================
CREATE TABLE marks (
    student_id INT,
    course_id INT,
    marks INT,
    FOREIGN KEY (student_id) REFERENCES students(student_id),
    FOREIGN KEY (course_id) REFERENCES courses(course_id)
);

-- =========================
-- 5. ATTENDANCE TABLE
-- =========================
CREATE TABLE attendance (
    student_id INT,
    course_id INT,
    attendance_percentage INT,
    FOREIGN KEY (student_id) REFERENCES students(student_id),
    FOREIGN KEY (course_id) REFERENCES courses(course_id)
);

-- =========================
-- 6. INSERT STUDENTS DATA
-- =========================
INSERT INTO students VALUES
(1, 'Ravi', 'CSE', 2),
(2, 'Anitha', 'CSE', 2),
(3, 'Kiran', 'ECE', 3),
(4, 'Sita', 'ECE', 1),
(5, 'Rahul', 'CSE', 4),
(6, 'Divya', 'IT', 3),
(7, 'Arjun', 'IT', 2),
(8, 'Priya', 'CSE', 1);

-- =========================
-- 7. INSERT COURSES DATA
-- =========================
INSERT INTO courses VALUES
(101, 'Database Management', 'Dr. Rao'),
(102, 'Machine Learning', 'Dr. Sharma'),
(103, 'Data Structures', 'Dr. Kumar'),
(104, 'Python Programming', 'Dr. Suresh');

-- =========================
-- 8. INSERT MARKS DATA
-- =========================
INSERT INTO marks VALUES
(1, 101, 85),
(1, 102, 78),
(2, 101, 90),
(2, 103, 88),
(3, 104, 75),
(3, 102, 80),
(4, 101, 70),
(4, 103, 60),
(5, 104, 95),
(5, 102, 82),
(6, 101, 89),
(6, 103, 91),
(7, 102, 76),
(7, 104, 84),
(8, 101, 65),
(8, 103, 72);

-- =========================
-- 9. INSERT ATTENDANCE DATA
-- =========================
INSERT INTO attendance VALUES
(1, 101, 85),
(1, 102, 80),
(2, 101, 90),
(2, 103, 88),
(3, 104, 75),
(3, 102, 82),
(4, 101, 70),
(4, 103, 68),
(5, 104, 95),
(5, 102, 89),
(6, 101, 92),
(6, 103, 90),
(7, 102, 78),
(7, 104, 85),
(8, 101, 65),
(8, 103, 70);

-- =========================
-- 10. ANALYSIS QUERIES
-- =========================

-- Average marks per student
SELECT student_id, AVG(marks) AS avg_marks
FROM marks
GROUP BY student_id;

-- Student performance with name
SELECT s.name, AVG(m.marks) AS avg_marks
FROM students s
JOIN marks m ON s.student_id = m.student_id
GROUP BY s.name;

-- Pass / Fail logic
SELECT s.name,
       AVG(m.marks) AS avg_marks,
       CASE
           WHEN AVG(m.marks) >= 40 THEN 'PASS'
           ELSE 'FAIL'
       END AS result
FROM students s
JOIN marks m ON s.student_id = m.student_id
GROUP BY s.name;

-- Department wise performance
SELECT s.department, AVG(m.marks) AS dept_avg
FROM students s
JOIN marks m ON s.student_id = m.student_id
GROUP BY s.department;

-- Attendance view
SELECT s.name, a.attendance_percentage
FROM students s
JOIN attendance a ON s.student_id = a.student_id;
