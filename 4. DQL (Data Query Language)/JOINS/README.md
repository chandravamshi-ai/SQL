### Introduction to Joins

Joins are used to retrieve data from two or more tables based on a related column. They allow you to create a relationship between tables and fetch data that is stored across these tables in a meaningful way.

### Types of Joins

1. **INNER JOIN**
2. **LEFT JOIN** (or LEFT OUTER JOIN)
3. **RIGHT JOIN** (or RIGHT OUTER JOIN)
4. **FULL JOIN** (or FULL OUTER JOIN)
5. **CROSS JOIN**
6. **SELF JOIN**

### Example Tables

#### `students` Table
| student_id | name   | age | grade | class_id |
|------------|--------|-----|-------|----------|
| 1          | Alice  | 15  | 10th  | 101      |
| 2          | Bob    | 16  | 10th  | 102      |
| 3          | Carol  | 16  | 11th  | 101      |
| 4          | David  | 17  | 12th  | 103      |
| 5          | Eve    | 15  | 11th  | 104      |

#### `classes` Table
| class_id | class_name | teacher    |
|----------|------------|------------|
| 101      | Math       | Mr. Smith  |
| 102      | Science    | Mrs. Brown |
| 103      | History    | Mr. White  |
| 105      | Art        | Ms. Green  |

## Detailed Explanation of Joins in DQL

### 1. INNER JOIN

**Purpose**: Returns only the rows where there is a match in both tables.

**Syntax**:
```sql
SELECT students.name, classes.class_name, classes.teacher
FROM students
INNER JOIN classes ON students.class_id = classes.class_id;
```

**Result**:
| name   | class_name | teacher    |
|--------|------------|------------|
| Alice  | Math       | Mr. Smith  |
| Bob    | Science    | Mrs. Brown |
| Carol  | Math       | Mr. Smith  |
| David  | History    | Mr. White  |

### Explanation:
- **INNER JOIN** returns rows that have matching values in both tables.

### 2. LEFT JOIN (or LEFT OUTER JOIN)

**Purpose**: Returns all rows from the left table, and the matched rows from the right table. If no match is found, NULL values are returned for columns from the right table.

**Syntax**:
```sql
SELECT students.name, classes.class_name, classes.teacher
FROM students
LEFT JOIN classes ON students.class_id = classes.class_id;
```

**Result**:
| name   | class_name | teacher    |
|--------|------------|------------|
| Alice  | Math       | Mr. Smith  |
| Bob    | Science    | Mrs. Brown |
| Carol  | Math       | Mr. Smith  |
| David  | History    | Mr. White  |
| Eve    | NULL       | NULL       |

### Explanation:
- **LEFT JOIN** returns all rows from the `students` table and matched rows from the `classes` table. Rows in the `students` table with no match in the `classes` table will show `NULL` for columns from the `classes` table.

### 3. RIGHT JOIN (or RIGHT OUTER JOIN)

**Purpose**: Returns all rows from the right table, and the matched rows from the left table. If no match is found, NULL values are returned for columns from the left table.

**Syntax**:
```sql
SELECT students.name, classes.class_name, classes.teacher
FROM students
RIGHT JOIN classes ON students.class_id = classes.class_id;
```

**Result**:
| name   | class_name | teacher    |
|--------|------------|------------|
| Alice  | Math       | Mr. Smith  |
| Bob    | Science    | Mrs. Brown |
| Carol  | Math       | Mr. Smith  |
| David  | History    | Mr. White  |
| NULL   | Art        | Ms. Green  |

### Explanation:
- **RIGHT JOIN** returns all rows from the `classes` table and matched rows from the `students` table. Rows in the `classes` table with no match in the `students` table will show `NULL` for columns from the `students` table.

### 4. FULL JOIN (or FULL OUTER JOIN)

**Purpose**: Returns all rows when there is a match in one of the tables. If there is no match, NULL values are returned for columns from the table without a match.

**Syntax**:
```sql
SELECT students.name, classes.class_name, classes.teacher
FROM students
FULL JOIN classes ON students.class_id = classes.class_id;
```

**Result**:
| name   | class_name | teacher    |
|--------|------------|------------|
| Alice  | Math       | Mr. Smith  |
| Bob    | Science    | Mrs. Brown |
| Carol  | Math       | Mr. Smith  |
| David  | History    | Mr. White  |
| Eve    | NULL       | NULL       |
| NULL   | Art        | Ms. Green  |

### Explanation:
- **FULL JOIN** returns all rows when there is a match in one of the tables. Rows with no match in the other table will show `NULL` for columns from the table without a match.

### 5. CROSS JOIN

**Purpose**: Returns the Cartesian product of the two tables, i.e., all possible combinations of rows.

**Syntax**:
```sql
SELECT students.name, classes.class_name
FROM students
CROSS JOIN classes;
```

**Result**:
| name   | class_name |
|--------|------------|
| Alice  | Math       |
| Alice  | Science    |
| Alice  | History    |
| Alice  | Art        |
| Bob    | Math       |
| Bob    | Science    |
| Bob    | History    |
| Bob    | Art        |
| Carol  | Math       |
| Carol  | Science    |
| Carol  | History    |
| Carol  | Art        |
| David  | Math       |
| David  | Science    |
| David  | History    |
| David  | Art        |
| Eve    | Math       |
| Eve    | Science    |
| Eve    | History    |
| Eve    | Art        |

### Explanation:
- **CROSS JOIN** produces a Cartesian product of the two tables. Every row from the `students` table is paired with every row from the `classes` table.

### 6. SELF JOIN

**Purpose**: To join a table with itself. Useful for comparing rows within the same table.

**Syntax**:
```sql
SELECT a.name AS student1, b.name AS student2
FROM students a
JOIN students b ON a.class_id = b.class_id
WHERE a.student_id < b.student_id;
```

**Result**:
| student1 | student2 |
|----------|----------|
| Alice    | Carol    |

### Explanation:
- **SELF JOIN** is joining the `students` table with itself using aliases (`a` and `b`).
- It finds pairs of students who are in the same class.

## Combining Joins with Other Clauses

### Filtering with WHERE Clause

**Example**: Find all students and their classes where the student is in the 10th grade.

```sql
SELECT students.name, classes.class_name, classes.teacher
FROM students
INNER JOIN classes ON students.class_id = classes.class_id
WHERE students.grade = '10th';
```

**Result**:
| name   | class_name | teacher    |
|--------|------------|------------|
| Alice  | Math       | Mr. Smith  |
| Bob    | Science    | Mrs. Brown |

### Grouping with GROUP BY Clause

**Example**: Count the number of students in each class.

```sql
SELECT classes.class_name, COUNT(students.student_id) AS student_count
FROM students
INNER JOIN classes ON students.class_id = classes.class_id
GROUP BY classes.class_name;
```

**Result**:
| class_name | student_count |
|------------|----------------|
| Math       | 2              |
| Science    | 1              |
| History    | 1              |

### Sorting with ORDER BY Clause

**Example**: List all students and their classes, sorted by class name.

```sql
SELECT students.name, classes.class_name
FROM students
INNER JOIN classes ON students.class_id = classes.class_id
ORDER BY classes.class_name;
```

**Result**:
| name   | class_name |
|--------|------------|
| Alice  | Math       |
| Carol  | Math       |
| Bob    | Science    |
| David  | History    |

## Summary

- **INNER JOIN**: Returns only the rows where there is a match in both tables.
- **LEFT JOIN**: Returns all rows from the left table, with matched rows from the right table.
- **RIGHT JOIN**: Returns all rows from the right table, with matched rows from the left table.
- **FULL JOIN**: Returns all rows when there is a match in either table.
- **CROSS JOIN**: Returns the Cartesian product of the two tables.
- **SELF JOIN**: Joins a table with itself for comparison within the same table.

Understanding these joins allows you to effectively query and retrieve data from multiple tables in a relational database, providing a comprehensive and flexible way to analyze and manipulate data.
