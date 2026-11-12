# Set-2
   1. STUDENT table – ALTER, INSERT and display
Create the table
CREATE TABLE STUDENT (
 STUDENT_ID NUMBER(5) PRIMARY KEY,
 STUDENT_NAME VARCHAR2(30),
 ADDRESS VARCHAR2(60),
 PHONE_NO VARCHAR2(15),
 DEPT_ID NUMBER(5),
 DEPT_NAME VARCHAR2(20),
 COURSE_ID CHAR(8),
 MARKS NUMBER(5,2)
);
a) Add DOB column
ALTER TABLE STUDENT ADD (DOB DATE);
b) Change COURSE_ID from CHAR to VARCHAR2
ALTER TABLE STUDENT MODIFY (COURSE_ID VARCHAR2(15));
c) Rename STUDENT_ID to S_ID
ALTER TABLE STUDENT RENAME COLUMN STUDENT_ID TO S_ID;
d) Modify STUDENT_NAME column width
ALTER TABLE STUDENT MODIFY (STUDENT_NAME VARCHAR2(50));
e) Insert five records and display all records
INSERT INTO STUDENT VALUES
(1,'Dustin','Hyderabad','9876500001',10,'CSE','DBMS',88,DATE '2004-05-12');
INSERT INTO STUDENT VALUES
(2,'Brutus','Warangal','9876500002',20,'ECE','JAVA',76,DATE '2004-08-21');
INSERT INTO STUDENT VALUES
(3,'Lubber','Hyderabad','9876500003',10,'CSE','PYTHON',91,DATE '2003-11-02');
INSERT INTO STUDENT VALUES
(4,'Andy','Karimnagar','9876500004',30,'IT','DBMS',82,DATE '2004-01-15');
INSERT INTO STUDENT VALUES
(5,'Rama','Nizamabad','9876500005',10,'CSE','SQL',79,DATE '2004-09-09');
SELECT * FROM STUDENT;
COMMIT;
II) Trigger for INSERT, UPDATE and DELETE
A trigger runs automatically when the specified event occurs. The following example stores old/new values in an audit
table.
CREATE TABLE STUDENT_AUDIT (
 S_ID NUMBER,
 ACTION_TYPE VARCHAR2(10), DBMS Lab External – Set 2 | Answers Page 3
 OLD_NAME VARCHAR2(50),
 NEW_NAME VARCHAR2(50),
 ACTION_DATE DATE
);
CREATE OR REPLACE TRIGGER TRG_STUDENT_AUDIT
AFTER INSERT OR UPDATE OR DELETE ON STUDENT
FOR EACH ROW
BEGIN
 IF INSERTING THEN
 INSERT INTO STUDENT_AUDIT
 VALUES (:NEW.S_ID, 'INSERT', NULL, :NEW.STUDENT_NAME, SYSDATE);
 ELSIF UPDATING THEN
 INSERT INTO STUDENT_AUDIT
 VALUES (:NEW.S_ID, 'UPDATE', :OLD.STUDENT_NAME,
 :NEW.STUDENT_NAME, SYSDATE);
 ELSIF DELETING THEN
 INSERT INTO STUDENT_AUDIT
 VALUES (:OLD.S_ID, 'DELETE', :OLD.STUDENT_NAME, NULL, SYSDATE);
 END IF;
END;
/
Test: INSERT a student, UPDATE the student's name, DELETE that student, then run SELECT * FROM
STUDENT_AUDIT;
   2. STUDENT – INSERT, SELECT, UPDATE and stored procedures
Create table and insert five rows
CREATE TABLE STUDENT (
 STUDENT_ID NUMBER(5) PRIMARY KEY,
 STUDENT_NAME VARCHAR2(30),
 ADDRESS VARCHAR2(60),
 PHONE_NO VARCHAR2(15),
 DEPT_ID NUMBER(5),
 DEPT_NAME VARCHAR2(20),
 COURSE_ID VARCHAR2(15),
 MARKS NUMBER(5,2)
);
INSERT INTO STUDENT VALUES (11,'Anu','Vijayawada','9000000011',10,'CSE','DBMS',90);
INSERT INTO STUDENT VALUES (12,'Ravi','Guntur','9000000012',10,'CSE','SQL',85);
INSERT INTO STUDENT VALUES (13,'Priya','Chennai','9000000013',20,'ECE','JAVA',88);
INSERT INTO STUDENT VALUES (14,'Arun','Nellore','9000000014',30,'IT','PYTHON',79);
INSERT INTO STUDENT VALUES (15,'Kiran','Hyderabad','9000000015',10,'CSE','DBMS',92);
a/b) Insert rows and display all information
SELECT * FROM STUDENT;
c) Update address of student 12 to Hyderabad
UPDATE STUDENT
SET ADDRESS = 'Hyderabad'
WHERE STUDENT_ID = 12;
COMMIT;
II) Stored procedure – create, execute and modify
CREATE OR REPLACE PROCEDURE UPDATE_STUDENT_MARKS (
 P_ID IN NUMBER,
 P_MARKS IN NUMBER
) AS
BEGIN
 UPDATE STUDENT
 SET MARKS = P_MARKS
 WHERE STUDENT_ID = P_ID;
END;
/
-- Execute the procedure
EXEC UPDATE_STUDENT_MARKS(12, 95);
-- Modify the procedure (replace its definition)
CREATE OR REPLACE PROCEDURE UPDATE_STUDENT_MARKS (
 P_ID IN NUMBER, DBMS Lab External – Set 2 | Answers Page 4
 P_MARKS IN NUMBER
) AS
BEGIN
 UPDATE STUDENT
 SET MARKS = P_MARKS
 WHERE STUDENT_ID = P_ID;
 COMMIT;
END;
/
A stored procedure is a named PL/SQL program stored in the database. CREATE OR REPLACE is used to create it or
replace its existing definition.
    3. EMPLOYEE queries and MongoDB CRUD
Create EMPLOYEE table
CREATE TABLE EMPLOYEE (
 EMP_NO NUMBER(5) PRIMARY KEY,
 E_NAME VARCHAR2(30),
 E_ADDRESS VARCHAR2(60),
 E_PH_NO VARCHAR2(15),
 DEPT_NO NUMBER(5),
 DEPT_NAME VARCHAR2(20),
 JOB_ID VARCHAR2(15),
 SALARY NUMBER(10,2),
 EMAIL_ID VARCHAR2(60)
);
a) Employees working in MECH department
SELECT * FROM EMPLOYEE WHERE UPPER(DEPT_NAME) = 'MECH';
b) Delete/clear email_id of employee James
UPDATE EMPLOYEE
SET EMAIL_ID = NULL
WHERE UPPER(E_NAME) = 'JAMES';
This sets James's email to NULL; it does not delete the employee record.
c) Complete records of SALES department employees
SELECT * FROM EMPLOYEE WHERE UPPER(DEPT_NAME) = 'SALES';
II) MongoDB CRUD operations on a collection
// Create or switch to database
use college
// CREATE: insert one document
db.student.insertOne({
 sid: 101, name: "Dustin", dept: "CSE", marks: 90
});
// CREATE: insert many documents
db.student.insertMany([
 {sid: 102, name: "Brutus", dept: "ECE", marks: 75},
 {sid: 103, name: "Lubber", dept: "CSE", marks: 85}
]);
// READ
db.student.find();
db.student.find({dept: "CSE"});
// UPDATE
db.student.updateOne(
 {sid: 102},
 {$set: {marks: 80}}
);
// DELETE
db.student.deleteOne({sid: 103});
CRUD means Create, Read, Update and Delete. DBMS Lab External – Set 2 | Answers Page 5
4. EMPLOYEE aggregate queries and MongoDB arrays/functions
SQL queries
-- a) Count employee names
SELECT COUNT(E_NAME) AS TOTAL_EMPLOYEES
FROM EMPLOYEE;
-- b) Maximum age (assuming AGE column exists)
SELECT MAX(AGE) AS MAX_AGE FROM EMPLOYEE;
-- c) Minimum age
SELECT MIN(AGE) AS MIN_AGE FROM EMPLOYEE;
II) MongoDB arrays, count, sort and aggregate
// Insert documents containing an array
db.student.insertMany([
 {sid: 1, name: "Anu", marks: 90, skills: ["SQL", "Java"]},
 {sid: 2, name: "Ravi", marks: 75, skills: ["Python", "MongoDB"]},
 {sid: 3, name: "Priya", marks: 85, skills: ["SQL", "Python"]}
]);
// Display documents where skills array contains SQL
db.student.find({skills: "SQL"});
// Count documents
db.student.countDocuments();
// Sort by marks ascending; use -1 for descending
db.student.find().sort({marks: 1});
db.student.find().sort({marks: -1});
// Aggregate: average marks department-wise
db.student.aggregate([
 {$group: {_id: "$dept", averageMarks: {$avg: "$marks"}}}
]);
// Aggregate: count students department-wise
db.student.aggregate([
 {$group: {_id: "$dept", totalStudents: {$sum: 1}}}
]);
Note: If the documents do not contain a dept field, add it or change $dept to a field that exists.
    5. SAILOR, RESERVES and BOATS – nested queries
Create tables (sample schema)
CREATE TABLE SAILOR (
 SID NUMBER PRIMARY KEY,
 SNAME VARCHAR2(30),
 RATING NUMBER,
 AGE NUMBER(4,1)
);
CREATE TABLE BOATS (
 BID NUMBER PRIMARY KEY,
 BNAME VARCHAR2(30),
 COLOR VARCHAR2(15)
);
CREATE TABLE RESERVES (
 SID NUMBER REFERENCES SAILOR(SID),
 BID NUMBER REFERENCES BOATS(BID),
 DAY DATE,
 PRIMARY KEY (SID, BID, DAY)
);
a) Names of sailors who reserved a red boat – nested query
SELECT SNAME
FROM SAILOR
WHERE SID IN (
 SELECT SID
 FROM RESERVES
 WHERE BID IN (
 SELECT BID FROM BOATS WHERE LOWER(COLOR) = 'red' DBMS Lab External – Set 2 | Answers Page 6
 )
);
b) Names of sailors who reserved boat number 103 – correlated nested query
SELECT S.SNAME
FROM SAILOR S
WHERE EXISTS (
 SELECT 1
 FROM RESERVES R
 WHERE R.SID = S.SID
 AND R.BID = 103
);
The inner query is correlated because it refers to S.SID from the outer SAILOR query.
     6. SAILOR aggregate queries and normalization
Create and populate SAILOR
CREATE TABLE SAILOR (
 SID NUMBER PRIMARY KEY,
 SNAME VARCHAR2(30),
 RATING NUMBER,
 AGE NUMBER(4,1)
);
INSERT INTO SAILOR VALUES (22,'Dustin',7,45.0);
INSERT INTO SAILOR VALUES (29,'Brutus',1,33.0);
INSERT INTO SAILOR VALUES (31,'Lubber',8,55.5);
INSERT INTO SAILOR VALUES (58,'Rusty',10,35.0);
INSERT INTO SAILOR VALUES (64,'Horatio',7,35.0);
INSERT INTO SAILOR VALUES (71,'Zorbas',10,16.0);
INSERT INTO SAILOR VALUES (85,'Art',3,25.5);
INSERT INTO SAILOR VALUES (95,'Bob',3,63.5);
a) Average age of sailors with rating 10
SELECT AVG(AGE) AS AVG_AGE FROM SAILOR WHERE RATING = 10;
Result: (35.0 + 16.0) / 2 = 25.5.
b) Minimum age
SELECT MIN(AGE) AS MIN_AGE FROM SAILOR;
Result: 16.0.
c) Maximum age
SELECT MAX(AGE) AS MAX_AGE FROM SAILOR;
Result: 63.5.
d) Number of sailors for each rating
SELECT RATING, COUNT(*) AS SAILOR_COUNT FROM SAILOR GROUP BY RATING ORDER BY RATING;
Expected counts: rating 1 → 1; rating 3 → 2; rating 7 → 2; rating 8 → 1; rating 10 → 2.
II) 1NF, 2NF and 3NF with examples
1NF (First Normal Form): Each cell contains one atomic value; no repeating groups or multi-valued columns. Example:
store one phone number per row rather than Phone1, Phone2 or a list in one cell.
STUDENT(S_ID, S_NAME, PHONE)
101, Anu, 9876500001
102, Ravi, 9876500002
2NF (Second Normal Form): The table is in 1NF and every non-key attribute depends on the whole candidate key, not
just part of a composite key. Example: ENROLLMENT(S_ID, COURSE_ID, S_NAME, COURSE_NAME, MARKS) has
composite key (S_ID, COURSE_ID). S_NAME depends only on S_ID and COURSE_NAME only on COURSE_ID. Split it:
STUDENT(S_ID, S_NAME)
COURSE(COURSE_ID, COURSE_NAME)
ENROLLMENT(S_ID, COURSE_ID, MARKS) DBMS Lab External – Set 2 | Answers Page 7
3NF (Third Normal Form): The table is in 2NF and has no transitive dependency of non-key attributes on a key. Example:
STUDENT(S_ID, DEPT_ID, DEPT_NAME), where S_ID → DEPT_ID and DEPT_ID → DEPT_NAME. Split it:
STUDENT(S_ID, DEPT_ID)
DEPARTMENT(DEPT_ID, DEPT_NAME)
   7. CUSTOMER and ORDERS – joins
Sample schema used because the question paper does not show the columns:
CREATE TABLE CUSTOMER (
 CUSTOMER_ID NUMBER PRIMARY KEY,
 CUSTOMER_NAME VARCHAR2(30)
);
CREATE TABLE ORDERS (
 ORDER_ID NUMBER PRIMARY KEY,
 CUSTOMER_ID NUMBER,
 ORDER_DATE DATE,
 FOREIGN KEY (CUSTOMER_ID) REFERENCES CUSTOMER(CUSTOMER_ID)
);
a) INNER JOIN – only matching customers and orders
SELECT C.CUSTOMER_ID, C.CUSTOMER_NAME, O.ORDER_ID, O.ORDER_DATE
FROM CUSTOMER C
INNER JOIN ORDERS O ON C.CUSTOMER_ID = O.CUSTOMER_ID;
b) LEFT OUTER JOIN – all customers, matching orders if any
SELECT C.CUSTOMER_ID, C.CUSTOMER_NAME, O.ORDER_ID, O.ORDER_DATE
FROM CUSTOMER C
LEFT OUTER JOIN ORDERS O ON C.CUSTOMER_ID = O.CUSTOMER_ID;
c) RIGHT OUTER JOIN – all orders, matching customer if any
SELECT C.CUSTOMER_ID, C.CUSTOMER_NAME, O.ORDER_ID, O.ORDER_DATE
FROM CUSTOMER C
RIGHT OUTER JOIN ORDERS O ON C.CUSTOMER_ID = O.CUSTOMER_ID;
   8. STUDENT and MARKS – INTERSECT, UNION ALL, EXISTS; procedures
Create tables and insert the given data
CREATE TABLE STUDENT (
 S_ID NUMBER PRIMARY KEY,
 S_NAME VARCHAR2(30),
 DEPT VARCHAR2(10)
);
CREATE TABLE MARKS (
 S_ID NUMBER,
 SUBJECT VARCHAR2(20),
 MARKS NUMBER(5,2)
);
INSERT INTO STUDENT VALUES (101,'Dustin','CSE');
INSERT INTO STUDENT VALUES (102,'Brutus','ECE');
INSERT INTO STUDENT VALUES (103,'Lubber','CSE');
INSERT INTO STUDENT VALUES (104,'Andy','IT');
INSERT INTO STUDENT VALUES (105,'Rama','CSE');
INSERT INTO MARKS VALUES (101,'DBMS',90);
INSERT INTO MARKS VALUES (102,'DBMS',65);
INSERT INTO MARKS VALUES (103,'DBMS',85);
INSERT INTO MARKS VALUES (104,'DBMS',55);
a) Student IDs scoring above 60 AND above 80 – INTERSECT
SELECT S_ID FROM MARKS WHERE MARKS > 60
INTERSECT
SELECT S_ID FROM MARKS WHERE MARKS > 80;
Result with the given marks: 101 and 103.
b) All IDs from both tables, including duplicates – UNION ALL
SELECT S_ID FROM STUDENT
UNION ALL DBMS Lab External – Set 2 | Answers Page 8
SELECT S_ID FROM MARKS;
c) Student names whose IDs exist in MARKS – EXISTS
SELECT S.S_NAME
FROM STUDENT S
WHERE EXISTS (
 SELECT 1 FROM MARKS M WHERE M.S_ID = S.S_ID
);
Result: Dustin, Brutus, Lubber and Andy.
II) Stored procedure example – create, execute and modify
CREATE OR REPLACE PROCEDURE ADD_MARKS (
 P_SID IN NUMBER,
 P_SUBJECT IN VARCHAR2,
 P_MARKS IN NUMBER
) AS
BEGIN
 INSERT INTO MARKS(S_ID, SUBJECT, MARKS)
 VALUES(P_SID, P_SUBJECT, P_MARKS);
END;
/
-- Execute
EXEC ADD_MARKS(105, 'DBMS', 78);
-- Modify by replacing the procedure definition
CREATE OR REPLACE PROCEDURE ADD_MARKS (
 P_SID IN NUMBER,
 P_SUBJECT IN VARCHAR2,
 P_MARKS IN NUMBER
) AS
BEGIN
 INSERT INTO MARKS(S_ID, SUBJECT, MARKS)
 VALUES(P_SID, P_SUBJECT, P_MARKS);
 COMMIT;
END;
/
   9. MongoDB STUDENT CRUD and SQL insert/display
I) MongoDB CRUD operations
use college
// CREATE
db.student.insertMany([
 {sid: 101, name: "Dustin", address: "Hyderabad", dept: "CSE", marks: 90},
 {sid: 102, name: "Brutus", address: "Warangal", dept: "ECE", marks: 75},
 {sid: 103, name: "Lubber", address: "Hyderabad", dept: "CSE", marks: 85},
 {sid: 104, name: "Andy", address: "Nellore", dept: "IT", marks: 80},
 {sid: 105, name: "Rama", address: "Guntur", dept: "CSE", marks: 88}
]);
// READ
db.student.find();
db.student.find({dept: "CSE"});
// UPDATE
db.student.updateOne({sid: 102}, {$set: {address: "Hyderabad"}});
// DELETE
db.student.deleteOne({sid: 105});
II) SQL STUDENT table – insert five rows and display
CREATE TABLE STUDENT (
 STUDENT_ID NUMBER PRIMARY KEY,
 STUDENT_NAME VARCHAR2(30),
 ADDRESS VARCHAR2(60),
 PHONE_NO VARCHAR2(15),
 DEPT_ID NUMBER,
 DEPT_NAME VARCHAR2(20),
 COURSE_ID VARCHAR2(15),
 MARKS NUMBER(5,2) DBMS Lab External – Set 2 | Answers Page 9
);
INSERT INTO STUDENT VALUES (1,'Dustin','Hyderabad','9876500001',10,'CSE','DBMS',90);
INSERT INTO STUDENT VALUES (2,'Brutus','Warangal','9876500002',20,'ECE','JAVA',75);
INSERT INTO STUDENT VALUES (3,'Lubber','Hyderabad','9876500003',10,'CSE','SQL',85);
INSERT INTO STUDENT VALUES (4,'Andy','Nellore','9876500004',30,'IT','PYTHON',80);
INSERT INTO STUDENT VALUES (5,'Rama','Guntur','9876500005',10,'CSE','DBMS',88);
SELECT * FROM STUDENT;
10. GROUP BY, ORDER BY, ANY, ALL and UNION
I) STUDENT table and given records
CREATE TABLE STUDENT (
 S_ID NUMBER PRIMARY KEY,
 S_NAME VARCHAR2(30),
 AGE NUMBER,
 MARKS NUMBER
);
INSERT INTO STUDENT VALUES (101,'Anu',22,90);
INSERT INTO STUDENT VALUES (102,'Shane',21,80);
INSERT INTO STUDENT VALUES (103,'Rohan',24,76);
INSERT INTO STUDENT VALUES (104,'Scott',23,100);
INSERT INTO STUDENT VALUES (105,'Tiger',22,80);
INSERT INTO STUDENT VALUES (106,'Alex',20,70);
INSERT INTO STUDENT VALUES (107,'Abhi',21,80);
a) Group students by marks
SELECT MARKS, COUNT(*) AS STUDENT_COUNT FROM STUDENT GROUP BY MARKS ORDER BY MARKS;
Expected groups: 70 → 1, 76 → 1, 80 → 3, 90 → 1, 100 → 1.
b) Marks in ascending order
SELECT S_NAME, MARKS FROM STUDENT ORDER BY MARKS ASC;
c) Marks in descending order
SELECT S_NAME, MARKS FROM STUDENT ORDER BY MARKS DESC;
II) STUDENT, ENROLLMENT and COURSE tables
CREATE TABLE STUDENT (
 S_ID NUMBER PRIMARY KEY,
 S_NAME VARCHAR2(30),
 MARKS NUMBER
);
INSERT INTO STUDENT VALUES (1,'Ravi',80);
INSERT INTO STUDENT VALUES (2,'Priya',90);
INSERT INTO STUDENT VALUES (3,'Arun',70);
CREATE TABLE COURSE (
 COURSE_ID NUMBER PRIMARY KEY,
 COURSE_NAME VARCHAR2(30)
);
CREATE TABLE ENROLLMENT (
 S_ID NUMBER REFERENCES STUDENT(S_ID),
 COURSE_ID NUMBER REFERENCES COURSE(COURSE_ID),
 ENROLL_DATE DATE,
 PRIMARY KEY(S_ID, COURSE_ID)
);
a) Names with marks greater than ANY marks below 90
SELECT S_NAME
FROM STUDENT
WHERE MARKS > ANY (
 SELECT MARKS FROM STUDENT WHERE MARKS < 90
);
For the given marks, the subquery returns 80 and 70. Greater than ANY means greater than at least one value, so Ravi
(80) and Priya (90) are returned.
b) Names with marks greater than ALL marks below 90 DBMS Lab External – Set 2 | Answers Page 10
SELECT S_NAME
FROM STUDENT
WHERE MARKS > ALL (
 SELECT MARKS FROM STUDENT WHERE MARKS < 90
);
Greater than ALL means greater than every value in the subquery. Result: Priya (90).
c) Student names scoring above 75 OR below 80 – UNION
SELECT S_NAME FROM STUDENT WHERE MARKS > 75
UNION
SELECT S_NAME FROM STUDENT WHERE MARKS < 80;
UNION removes duplicate names. With these marks, all three students are returned.
11. PL/SQL trigger and normalization
I) Trigger to audit INSERT, UPDATE and DELETE on STUDENT
CREATE TABLE STUDENT_LOG (
 S_ID NUMBER,
 OPERATION VARCHAR2(10),
 OLD_MARKS NUMBER,
 NEW_MARKS NUMBER,
 LOG_DATE DATE
);
CREATE OR REPLACE TRIGGER STUDENT_CHANGE_TRIGGER
AFTER INSERT OR UPDATE OR DELETE ON STUDENT
FOR EACH ROW
BEGIN
 IF INSERTING THEN
 INSERT INTO STUDENT_LOG
 VALUES (:NEW.S_ID, 'INSERT', NULL, :NEW.MARKS, SYSDATE);
 ELSIF UPDATING THEN
 INSERT INTO STUDENT_LOG
 VALUES (:NEW.S_ID, 'UPDATE', :OLD.MARKS, :NEW.MARKS, SYSDATE);
 ELSIF DELETING THEN
 INSERT INTO STUDENT_LOG
 VALUES (:OLD.S_ID, 'DELETE', :OLD.MARKS, NULL, SYSDATE);
 END IF;
END;
/
II) 1NF, 2NF and 3NF: refer to Question 6, Part II. The same definitions and decomposition examples apply.
12. MongoDB arrays, count, sort, aggregate and normalization
I) MongoDB implementation
// Array field
db.student.insertMany([
 {sid: 1, name: "Anu", marks: 90, subjects: ["DBMS", "SQL"]},
 {sid: 2, name: "Ravi", marks: 75, subjects: ["Java", "Python"]},
 {sid: 3, name: "Priya", marks: 85, subjects: ["DBMS", "MongoDB"]}
]);
// Array query
db.student.find({subjects: "DBMS"});
// Count
db.student.countDocuments({});
// Sort ascending and descending
db.student.find().sort({marks: 1});
db.student.find().sort({marks: -1});
// Aggregate: average marks
db.student.aggregate([
 {$group: {_id: null, averageMarks: {$avg: "$marks"}}}
]);
// Aggregate: count records
db.student.aggregate([ DBMS Lab External – Set 2 | Answers Page 11
 {$group: {_id: null, totalStudents: {$sum: 1}}}
]);
II) 1NF, 2NF and 3NF
1NF: every field contains a single atomic value and there are no repeating groups. 2NF: in 1NF, with every non-key
attribute fully dependent on the complete candidate key. 3NF: in 2NF, with no transitive dependency of non-key attributes
on a candidate key.
Examples and decompositions are provided in Question 6, Part II.
Quick viva revision
Q. What is DDL?
Data Definition Language; commands include CREATE, ALTER, DROP and TRUNCATE.
Q. What is DML?
Data Manipulation Language; commands include INSERT, UPDATE and DELETE.
Q. What is DQL?
Data Query Language; SELECT retrieves data.
Q. What is a primary key?
A column or set of columns that uniquely identifies each row and cannot contain NULL.
Q. What is a foreign key?
A column that refers to a primary or unique key in another table.
Q. What is a join?
A query operation that combines rows from two or more tables using a related column.
Q. What is a trigger?
A stored PL/SQL block that executes automatically when a specified database event occurs.
Q. What is a procedure?
A named PL/SQL program stored in the database and executed when called.
Q. What is normalization?
Organizing tables to reduce redundancy and avoid insertion, update and deletion anomalies.
Q. What is MongoDB?
A document-oriented NoSQL database that stores data in BSON documents.
