# HA150

### Exercise 2 - Read from a Single Table Using the SELECT Statement
```
SELECT *
 FROM Employee;
 
SELECT *
 FROM Employee
 WHERE RemainderDays < 30; 
 
SELECT DISTINCT DepID
 FROM Employee; 
 
SELECT *
 FROM Employee
 WHERE RemainderDays < 30
 ORDER BY RemainderDays DESC, DNumber ASC; 
 
 SELECT *
  FROM Employee
  WHERE RemainderDays IS NULL;
  
 SELECT DISTINCT RemainderDays
 FROM Employee
 WHERE RemainderDays IS NOT NULL;  
  
 SELECT DNumber AS "Personal Number", Name
 FROM Employee;  
  
 SELECT RemainderDays
 FROM Employee
 WHERE NAME = 'Ms D'; 
 
 SELECT *
 FROM Employee
 WHERE RemainderDays = 10;
 
 SELECT *, RemainderDays * 300 as RemainderDaysValue
 FROM Employee
 WHERE RemainderDays * 300 >= 600;
 
 SELECT *
 FROM Employee
 WHERE DepID = 'A01' or DepID = 'A04';
 
 SELECT *
 FROM Employee
 WHERE DepID IN ('A01', 'A04');
 
 SELECT *
 FROM Employee
 WHERE DepID <> 'A01' AND RemainderDays = 10;
 
 SELECT *
 FROM Employee
 WHERE  Name LIKE '%L%';
 
 SELECT *
 FROM Employee
 WHERE  Name LIKE 'M__K%';
```
## Exercise 3 - Understand NULL Values
```
SELECT *
 FROM Official;
 
SELECT *
 FROM Official
 WHERE Overtime < 20
    UNION
 SELECT *
 FROM Official
 WHERE Overtime = 20
    UNION
 SELECT * 
FROM Official
 WHERE Overtime > 20;
```
## Exercise 4 - Use Aggregate Expressions 
```
SELECT COUNT(*)
FROM Employee;

SELECT COUNT(*)
FROM Employee
WHERE RemainderDays = 20;

SELECT COUNT(DISTINCT DepID)
FROM Employee;

SELECT MAX (RemainderDays)
 FROM Employee;

SELECT AVG (RemainderDays)
 FROM Employee;
 
SELECT MIN (RemainderDays) as "min", AVG (RemainderDays) as "avg", 
       MAX (RemainderDays) as "max", SUM (RemainderDays) as "sum"
 FROM Employee;
 
SELECT MAX (RemainderDays) - AVG (RemainderDays)
 FROM Employee;
 
SELECT DepID, SUM (RemainderDays)
 FROM Employee
 GROUP BY DepID;

SELECT DepID, SUM (RemainderDays)
 FROM Employee
 GROUP BY DepID
 HAVING SUM (RemainderDays) > 30
 ORDER BY 2 DESC;
```
## Exercise 5 - Use JOIN to Combine Data from Several Tables
```
SELECT *
 FROM Employee
 WHERE DepID = 'A01'
 
 UNION
 
SELECT *
 FROM Employee
 WHERE DepID = 'A04';
 
SELECT emp.Name "Employee", dep.Name "Department"
 FROM Employee emp JOIN Department dep
    ON emp.DepID = dep.DepID; 
    
SELECT emp.Name "Employee", dep.Name "Department"
 FROM Employee emp LEFT OUTER JOIN Department dep
    ON emp.DepID = dep.DepID;
    
SELECT emp.Name "Employee", dep.Name "Department"
 FROM Employee emp RIGHT OUTER JOIN Department dep
    ON emp.DepID = dep.DepID;   

SELECT emp.Name "Employee", dep.Name "Department"
 FROM Employee emp FULL OUTER JOIN Department dep
    ON emp.DepID = dep.DepID; 
    
SELECT emp.*
 FROM Employee emp JOIN Department dep
    ON emp.DepID = dep.DepID
 WHERE dep.name = 'Sales';    
 
 SELECT dep.Name, SUM (RemainderDays) as DepartmentsVacation
 FROM Employee emp JOIN Department dep
    ON emp.DepID = dep.DepID
 WHERE DateOfBirth > '1972-02-03'
 GROUP BY dep.Name, dep.DepID
 HAVING SUM (RemainderDays) > 30
 ORDER BY DepartmentsVacation DESC; 
```
## Exercise 6 - Insert, Update, and Delete Rows in a Database Table
```
INSERT INTO STAFF
 SELECT *
 FROM Employee;
 
UPDATE STAFF
 SET RemainderDays = 50
 WHERE Name = 'Mr A';
 
UPDATE STAFF
 SET RemainderDays = 18
 WHERE RemainderDays > 18; 
 
UPDATE STAFF
 SET RemainderDays = RemainderDays * 3
 WHERE DepID IN
    (SELECT DepID
     FROM Department
     WHERE Name IN ('Sales', 'Test')); 
     
INSERT INTO STAFF
 VALUES ('D200000', 'Mr X', '1977-07-07', 10, 'A01');     
    
DELETE FROM STAFF
 WHERE Name = 'Mr A';    
     
DELETE FROM STAFF
 WHERE DepID IS NULL;  
 
DELETE FROM STAFF
 WHERE DepID IN
    (SELECT DepID
     FROM Department
     WHERE Name IN ('Planning', 'Test')); 
```
## Exercise 7 - Create and Call Scalar User-Defined Functions 
```
SELECT DNumber, "HA150::Remainderdays_Fixedvalue"(RemainderDays)
  FROM "HA150::EMPLOYEE"; 

SELECT DNumber, "HA150::RemainderDays_Variablevalue"(RemainderDays, DepID) FROM "HA150::EMPLOYEE";
```
## Exercise 15 - Monitor Suboptimal SQL 
```
select TOTAL_EXECUTION_TIME as "Duration", STATEMENT_STRING as "BLOCKER", * 
from "SYS"."M_SQL_PLAN_CACHE" 
where SCHEMA_Name = 'STUDENT##'
order by TOTAL_EXECUTION_TIME desc; 

--M_EXPENSIVE_STATEMENTS Select 
SELECT "STATEMENT_STRING","OBJECT_NAME","DURATION_MICROSEC", *
FROM "SYS"."M_EXPENSIVE_STATEMENTS" where db_user = 'STUDENT20'

--Checking Activation Status of Exepensive Statement Tracing
SELECT "FILE_NAME","LAYER_NAME","SECTION","KEY","VALUE"
FROM "SYS"."M_INIFILE_CONTENTS"
where "SECTION" like 'expen%';

--Activate Exepensive Statement Tracing
ALTER SYSTEM ALTER CONFIGURATION ('global.ini','SYSTEM') 
SET ('expensive_statement','enable') = 'TRUE'
WITH RECONFIGURE;


SELECT "FILE_NAME","LAYER_NAME","SECTION","KEY","VALUE"
FROM "SYS"."M_INIFILE_CONTENTS"
where "SECTION" like 'trace' and key like 'trac%'; ;


ALTER SYSTEM ALTER CONFIGURATION ('indexserver.ini','SYSTEM') 
SET ('traceprofile_MyTrace##','sql_user') = 'STUDENT20', 
('traceprofile_MyTrace##','sqlopttime') = 'debug'
 WITH RECONFIGURE;

 
 --Stored Procedure Call - if execution plan exists renew it
call "my_sql_challenge" ('01.01.2014','23.02.2022',?) with hint ( IGNORE_PLAN_CACHE );

 
ALTER SYSTEM ALTER CONFIGURATION ('indexserver.ini','SYSTEM') 
UNSET ('traceprofile_MyTrace##','sql_user') , 
('traceprofile_MyTrace##','sqloptime')
 WITH RECONFIGURE;
```
## Exercise 27 - Use Nested Queries 
```
SELECT *
 FROM Employee
 WHERE DepID IN
    (SELECT DepID
        FROM Department
        WHERE Name = 'Sales');
        
 SELECT *
 FROM Employee
 WHERE RemainderDays = 
    (SELECT MAX (RemainderDays)
        FROM Employee);        
        
SELECT *
 FROM Employee
 WHERE RemainderDays > 
    (SELECT AVG (RemainderDays)
        FROM Employee);        
```    
## Exercise 28 - Create, Change, and Delete a Database Table 
```
CREATE COLUMN TABLE Locations (
   Token   CHAR(3) PRIMARY KEY,
   Region  NVARCHAR(20) DEFAULT 'EMEA',
   Country NVARCHAR(20),
   City    NVARCHAR(20) NOT NULL);
   
 CREATE COLUMN TABLE EmployeeSalary(
   DNumber        NVARCHAR(7),
   FromDate       DATE,
   FixedSalary    DECIMAL(9,2) NOT NULL,
   VariableSalary DECIMAL(5,2) NOT NULL DEFAULT 5.0,
   PRIMARY KEY(DNumber,FromDate) );
   
 INSERT INTO EmployeeSalary
     VALUES ('D100001', current_date, 2500.0, 5.0 );
 INSERT INTO EmployeeSalary
     VALUES ('D100002', current_date, 3000.0, 15.0);
 INSERT INTO EmployeeSalary
     VALUES ('D100003', current_date, 3200.0, 17.0);   
     
ALTER TABLE LOCATIONS ADD (Population INTEGER, Mayor VARCHAR(30));     
     
ALTER TABLE LOCATIONS DROP (Country, Population);  

ALTER TABLE LOCATIONS ALTER (City VARCHAR(20) NULL);

DROP TABLE LOCATIONS;

DROP TABLE EMPLOYEESALARY;
```
## Exercise 29 - Work with Database Views  
```
CREATE VIEW ManagerView
 AS 
    SELECT DNumber, Name, RemainderDays AS VacationDays
      FROM Employee
     WHERE DepID = 'A04';
     
DROP VIEW ManagerView;     
```
