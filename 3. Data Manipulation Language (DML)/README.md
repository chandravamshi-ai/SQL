Let's dive deep into Data Manipulation Language (DML) in SQL. DML commands are used to manage data within database tables. We'll start with the basics and gradually move to intermediate and advanced levels, providing clear explanations and examples along the way.

### Data Manipulation Language (DML) Overview

DML commands are used to manipulate the data stored in database tables. The primary DML commands are:
1. **INSERT**: Add new records to a table.
2. **UPDATE**: Modify existing records in a table.
3. **DELETE**: Remove records from a table.
4. **SELECT**: Retrieve data from a table (often considered part of DQL - Data Query Language, but crucial in understanding DML).

Let's explore these commands step by step.

## Beginner Level: Fundamental DML Commands

### 1. **INSERT**

**Purpose**: To add new records to a table.

**Syntax**:
```sql
INSERT INTO table_name (column1, column2, ...) 
VALUES (value1, value2, ...);
```

**Example**:
```sql
INSERT INTO students (id, name, age, grade) 
VALUES (1, 'John Doe', 15, '10th');
```

**Explanation**:
- `students`: Table name where data is being inserted.
- `(id, name, age, grade)`: Columns into which the data is inserted.
- `(1, 'John Doe', 15, '10th')`: Values to be inserted into the respective columns.

### 2. **UPDATE**

**Purpose**: To modify existing records in a table.

**Syntax**:
```sql
UPDATE table_name 
SET column1 = value1, column2 = value2, ... 
WHERE condition;
```

**Example**:
```sql
UPDATE students 
SET age = 16, grade = '11th' 
WHERE id = 1;
```

**Explanation**:
- `students`: Table name where data is being updated.
- `SET age = 16, grade = '11th'`: Columns and their new values.
- `WHERE id = 1`: Condition to specify which record(s) to update.

### 3. **DELETE**

**Purpose**: To remove records from a table.

**Syntax**:
```sql
DELETE FROM table_name 
WHERE condition;
```

**Example**:
```sql
DELETE FROM students 
WHERE id = 1;
```

**Explanation**:
- `students`: Table name from which data is being deleted.
- `WHERE id = 1`: Condition to specify which record(s) to delete.

### 4. **SELECT**

**Purpose**: To retrieve data from a table.

**Syntax**:
```sql
SELECT column1, column2, ... 
FROM table_name 
WHERE condition;
```

**Example**:
```sql
SELECT name, age 
FROM students 
WHERE grade = '10th';
```

**Explanation**:
- `SELECT name, age`: Columns to retrieve.
- `FROM students`: Table name from which data is being retrieved.
- `WHERE grade = '10th'`: Condition to filter the records.

## Intermediate Level: Advanced Usage of DML Commands

### 1. **INSERT INTO SELECT**

**Purpose**: To insert data from one table into another.

**Syntax**:
```sql
INSERT INTO table_name (column1, column2, ...) 
SELECT column1, column2, ... 
FROM another_table 
WHERE condition;
```

**Example**:
```sql
INSERT INTO graduates (id, name, age, grade) 
SELECT id, name, age, grade 
FROM students 
WHERE grade = '12th';
```

**Explanation**:
- `graduates`: Destination table where data is being inserted.
- `students`: Source table from which data is being selected.
- `WHERE grade = '12th'`: Condition to filter the records in the source table.

### 2. **UPDATE with JOIN**

**Purpose**: To update records in a table based on a join with another table.

**Syntax**:
```sql
UPDATE table1 
SET table1.column = value 
FROM table1 
JOIN table2 
ON table1.common_column = table2.common_column 
WHERE condition;
```

**Example**:
```sql
UPDATE students 
SET students.grade = 'Graduated' 
FROM students 
JOIN graduates 
ON students.id = graduates.id 
WHERE graduates.year = 2023;
```

**Explanation**:
- `students`: Table name where data is being updated.
- `JOIN graduates ON students.id = graduates.id`: Join condition between the two tables.
- `WHERE graduates.year = 2023`: Condition to filter the records.

### 3. **DELETE with JOIN**

**Purpose**: To delete records from a table based on a join with another table.

**Syntax**:
```sql
DELETE table1 
FROM table1 
JOIN table2 
ON table1.common_column = table2.common_column 
WHERE condition;
```

**Example**:
```sql
DELETE students 
FROM students 
JOIN graduates 
ON students.id = graduates.id 
WHERE graduates.year = 2023;
```

**Explanation**:
- `students`: Table name from which data is being deleted.
- `JOIN graduates ON students.id = graduates.id`: Join condition between the two tables.
- `WHERE graduates.year = 2023`: Condition to filter the records.

### 4. **SELECT with JOINs and Aggregations**

**Purpose**: To retrieve data using joins and aggregate functions.

**Syntax**:
```sql
SELECT column1, column2, ..., AGGREGATE_FUNCTION(column) 
FROM table1 
JOIN table2 
ON table1.common_column = table2.common_column 
WHERE condition 
GROUP BY column1, column2 
HAVING condition 
ORDER BY column1, column2;
```

**Example**:
```sql
SELECT students.grade, COUNT(*) as student_count 
FROM students 
JOIN classes 
ON students.class_id = classes.id 
WHERE classes.teacher = 'Mr. Smith' 
GROUP BY students.grade 
HAVING COUNT(*) > 5 
ORDER BY student_count DESC;
```

**Explanation**:
- `JOIN classes ON students.class_id = classes.id`: Join condition between the two tables.
- `GROUP BY students.grade`: Grouping the results by grade.
- `HAVING COUNT(*) > 5`: Filtering groups with more than 5 students.
- `ORDER BY student_count DESC`: Ordering the results by student count in descending order.

## Advanced Level: Complex DML Operations

### 1. **Subqueries**

**Purpose**: To use the result of one query inside another query.

**Syntax**:
```sql
SELECT column1, column2, ... 
FROM table 
WHERE column IN (SELECT column FROM another_table WHERE condition);
```

**Example**:
```sql
SELECT name, age 
FROM students 
WHERE id IN (SELECT student_id FROM honors WHERE gpa > 3.5);
```

**Explanation**:
- The subquery `(SELECT student_id FROM honors WHERE gpa > 3.5)` returns a list of student IDs.
- The main query uses this list to filter students.

### 2. **Common Table Expressions (CTEs)**

**Purpose**: To simplify complex queries by breaking them into smaller, more manageable parts.

**Syntax**:
```sql
WITH cte_name AS (
    SELECT column1, column2, ... 
    FROM table 
    WHERE condition
)
SELECT column1, column2, ... 
FROM cte_name 
WHERE condition;
```

**Example**:
```sql
WITH student_grades AS (
    SELECT student_id, AVG(grade) as avg_grade 
    FROM grades 
    GROUP BY student_id
)
SELECT students.name, student_grades.avg_grade 
FROM students 
JOIN student_grades 
ON students.id = student_grades.student_id 
WHERE student_grades.avg_grade > 3.0;
```

**Explanation**:
- The CTE `student_grades` calculates the average grade for each student.
- The main query uses this CTE to find students with an average grade greater than 3.0.

### 3. **Transactions**

**Purpose**: To ensure a series of DML operations are executed as a single unit of work.

**Syntax**:
```sql
BEGIN TRANSACTION;
-- DML operations
COMMIT;
-- or
ROLLBACK;
```

**Example**:
```sql
BEGIN TRANSACTION;

UPDATE students 
SET grade = 'Graduated' 
WHERE id = 1;

DELETE FROM enrollments 
WHERE student_id = 1;

COMMIT;
-- or ROLLBACK if something goes wrong
```

**Explanation**:
- `BEGIN TRANSACTION`: Starts the transaction.
- `COMMIT`: Saves the changes.
- `ROLLBACK`: Reverts the changes if an error occurs.

### Summary of DML Commands

- **INSERT**: Add new records to a table.
- **UPDATE**: Modify existing records in a table.
- **DELETE**: Remove records from a table.
- **SELECT**: Retrieve data from a table.

### Advanced Techniques

- **INSERT INTO SELECT**: Insert data from one table to another.
- **UPDATE with JOIN**: Update records based on another table.
- **DELETE with JOIN**: Delete records based on another table.
- **SELECT with JOINs and Aggregations**: Retrieve data with joins and aggregate functions.
- **Subqueries**: Use query results within another query.
- **Common Table Expressions (CTEs)**: Simplify complex queries.
- **Transactions**: Ensure a series of DML operations are executed together.

By understanding these DML commands and techniques, you can effectively manage and manipulate data in relational databases, from basic operations to advanced and complex queries.
