# Important Interview Questions (Must know concepts)


### **Common Table Expressions (CTEs)**

A **Common Table Expression (CTE)** is a temporary result set in SQL that can be referred to within a `SELECT`, `INSERT`, `UPDATE`, or `DELETE` statement. It improves code readability, simplifies complex queries, and is especially useful for recursive queries.



### **Definition**
- **CTE Syntax:**
  ```sql
  WITH cte_name AS (
      -- Your SQL Query Here
  )
  SELECT * FROM cte_name;
  ```
  - `WITH`: Introduces the CTE.
  - `cte_name`: A temporary name for the result set.
  - **Scope:** The CTE exists only for the duration of the query it is defined in.



### **Why Use CTEs?**
1. **Improves Readability:** Breaks down complex queries into manageable pieces.
2. **Reusability:** CTEs can be referred to multiple times within the same query.
3. **Recursive Queries:** Enables operations like hierarchical data traversal (e.g., organizational charts or parent-child relationships).
4. **Alternative to Subqueries:** Sometimes easier to read and maintain than nested subqueries.



### **Example: Non-Recursive CTE**

#### **Scenario: Simplifying a Query**
- A table `Sales` has columns: `Region`, `Salesperson`, `SalesAmount`. You want to find total sales per region and then filter regions with total sales over $500.

#### **Query Without CTE:**
```sql
SELECT Region
FROM (
    SELECT Region, SUM(SalesAmount) AS TotalSales
    FROM Sales
    GROUP BY Region
) Subquery
WHERE TotalSales > 500;
```

#### **Query With CTE:**
```sql
WITH RegionalSales AS (
    SELECT Region, SUM(SalesAmount) AS TotalSales
    FROM Sales
    GROUP BY Region
)
SELECT Region
FROM RegionalSales
WHERE TotalSales > 500;
```

- **Result:** Both queries return the same result, but the CTE version is cleaner and easier to debug.



### **Recursive CTE**

Recursive CTEs are used to work with hierarchical or tree-structured data, such as organizational charts or folder structures.

#### **Scenario: Organizational Hierarchy**
- A table `Employees` has columns: `EmployeeID`, `ManagerID`, `EmployeeName`.
- **Task:** Find all employees under a specific manager (e.g., Manager with `EmployeeID = 1`).

#### **Query:**
```sql
WITH EmployeeHierarchy AS (
    -- Anchor Member: Start with the top-level manager
    SELECT EmployeeID, ManagerID, EmployeeName, 0 AS Level
    FROM Employees
    WHERE ManagerID = 1
    
    UNION ALL
    
    -- Recursive Member: Find direct reports of employees already in the hierarchy
    SELECT E.EmployeeID, E.ManagerID, E.EmployeeName, EH.Level + 1
    FROM Employees E
    INNER JOIN EmployeeHierarchy EH ON E.ManagerID = EH.EmployeeID
)
SELECT EmployeeID, EmployeeName, Level
FROM EmployeeHierarchy;
```

- **Result:**
  - Returns a list of employees under the specified manager, along with their levels in the hierarchy.



### **CTE vs Subquery**
| **Aspect**            | **CTE**                                      | **Subquery**                              |
||-|-|
| **Readability**        | Easier to read and maintain.                 | Can become hard to read when nested.      |
| **Reusability**        | Can reference the CTE multiple times.        | Must repeat the subquery if needed again. |
| **Performance**        | Similar performance in most cases.           | Can be harder to optimize in complex cases. |
| **Recursion Support**  | Supports recursion.                          | Does not support recursion.               |



### **Common Interview Questions About CTEs**

1. **What are CTEs, and why are they used?**
   - Definition: Temporary result sets to simplify queries.
   - Usage: Simplifies complex queries, enhances readability, supports recursion.

2. **What is the difference between CTEs and temporary tables?**
   - **CTEs:** Exists only during the query execution. They are more lightweight and used for readability.
   - **Temporary Tables:** Physically stored in the database for the session, used when intermediate results are reused across multiple queries.

3. **Can CTEs be used in UPDATE/DELETE/INSERT statements?**
   - Yes, they can be used in all these cases. Example:
     ```sql
     WITH SalesToUpdate AS (
         SELECT SalesID, SalesAmount
         FROM Sales
         WHERE SalesAmount < 100
     )
     UPDATE Sales
     SET SalesAmount = 100
     WHERE SalesID IN (SELECT SalesID FROM SalesToUpdate);
     ```

4. **How do recursive CTEs work?**
   - They have two parts:
     1. **Anchor Query:** Provides the starting rows.
     2. **Recursive Query:** Refers to itself to retrieve related rows iteratively.
   - Example: Hierarchical data, such as finding all subordinates under a manager.

5. **Can you use multiple CTEs in a single query?**
   - Yes, multiple CTEs can be chained or referenced.
   ```sql
   WITH CTE1 AS (
       SELECT ...
   ),
   CTE2 AS (
       SELECT ...
       FROM CTE1
   )
   SELECT ...
   FROM CTE2;
   ```

6. **What is the performance impact of using CTEs?**
   - Typically, CTEs perform similarly to equivalent subqueries.
   - Performance depends on how the SQL engine optimizes the query.

7. **Can you nest or reference a CTE inside another CTE?**
   - Yes, you can reference one CTE within another.

8. **How does a CTE differ from a view?**
   - **CTE:** Temporary, only exists during the query execution.
   - **View:** Persistent, stored in the database schema for reuse.

---


### **Subqueries in SQL**

A **subquery** (or inner query) is a query nested within another query. It allows you to dynamically fetch data to be used in the outer query. Subqueries can be used in `SELECT`, `FROM`, `WHERE`, or other clauses.



### **Why Use Subqueries?**
1. **Dynamic Filtering:** Fetch data for filters dynamically.
2. **Modular Queries:** Break complex logic into manageable parts.
3. **Intermediate Results:** Provide data that the outer query can reference.



### **Types of Subqueries**
Subqueries can be classified based on where they are used and how they return results.

#### **1. Based on Location**
- **In the `WHERE` Clause:** Filter rows dynamically.
- **In the `FROM` Clause:** Use the subquery as a derived table.
- **In the `SELECT` Clause:** Use the subquery to calculate values dynamically.

#### **2. Based on Result Type**
- **Single-Row Subqueries:** Return one row with one column.
- **Multi-Row Subqueries:** Return multiple rows (usually one column).
- **Multi-Column Subqueries:** Return multiple columns (can be used in `IN` or `EXISTS`).
- **Correlated Subqueries:** Refer to columns from the outer query, executing once for each row in the outer query.



### **Examples**

#### **1. Subquery in `WHERE` Clause**
Find employees who earn more than the average salary.

```sql
SELECT EmployeeName, Salary
FROM Employees
WHERE Salary > (SELECT AVG(Salary) FROM Employees);
```

- **Explanation:**
  - Inner query calculates the average salary.
  - Outer query selects employees whose salaries are above the average.


#### **2. Subquery in `FROM` Clause**
Find regions and the total sales in each region.

```sql
SELECT Region, TotalSales
FROM (
    SELECT Region, SUM(SalesAmount) AS TotalSales
    FROM Sales
    GROUP BY Region
) AS RegionalSales
WHERE TotalSales > 500;
```

- **Explanation:**
  - Inner query calculates the total sales per region.
  - Outer query filters regions with sales greater than 500.



#### **3. Subquery in `SELECT` Clause**
Add the average salary to each employee's details.

```sql
SELECT EmployeeName, Salary, 
       (SELECT AVG(Salary) FROM Employees) AS AverageSalary
FROM Employees;
```

- **Explanation:**
  - Subquery calculates the average salary.
  - Outer query appends the average salary to each employee’s details.



#### **4. Correlated Subquery**
Find employees whose salaries are higher than the average salary of their department.

```sql
SELECT EmployeeName, Salary, DepartmentID
FROM Employees E1
WHERE Salary > (
    SELECT AVG(Salary)
    FROM Employees E2
    WHERE E1.DepartmentID = E2.DepartmentID
);
```

- **Explanation:**
  - Inner query calculates the average salary for the department of the current employee (`E1`).
  - Outer query selects employees with salaries above this average.



#### **5. `IN` Subquery**
Find employees who work in departments located in 'New York'.

```sql
SELECT EmployeeName, DepartmentID
FROM Employees
WHERE DepartmentID IN (
    SELECT DepartmentID
    FROM Departments
    WHERE Location = 'New York'
);
```

- **Explanation:**
  - Subquery retrieves department IDs located in 'New York'.
  - Outer query filters employees working in those departments.


#### **6. `EXISTS` Subquery**
Find departments that have employees.

```sql
SELECT DepartmentID, DepartmentName
FROM Departments D
WHERE EXISTS (
    SELECT 1
    FROM Employees E
    WHERE D.DepartmentID = E.DepartmentID
);
```

- **Explanation:**
  - Subquery checks if there are employees in a department.
  - Outer query selects only departments that have employees.



### **Subquery vs JOIN**
| **Aspect**              | **Subquery**                                      | **JOIN**                                      |
|--------------------------|--------------------------------------------------|----------------------------------------------|
| **Use Case**             | Use for nested or dependent logic.               | Use for combining related tables.            |
| **Performance**          | Often slower, especially for correlated subqueries. | Generally faster due to direct table joins.  |
| **Readability**          | Easier for smaller, modular logic.               | Better for complex relationships.            |
| **Result Structure**     | Often single-column/row results.                 | Combines multiple columns from related tables.|



### **Common Interview Questions About Subqueries**

1. **What is a subquery, and where can it be used?**
   - A query nested within another query.
   - Can be used in `WHERE`, `FROM`, `SELECT`, `HAVING`, or other clauses.

2. **What is a correlated subquery?**
   - A subquery that depends on the outer query for its values.
   - Executes once for each row in the outer query.

3. **What is the difference between a subquery and a JOIN?**
   - Subquery: Used for dynamic filters or intermediate results.
   - JOIN: Used to combine data from related tables.

4. **When would you use a subquery over a JOIN?**
   - When you need modular logic or intermediate filtering that isn't easily achievable with JOINs.

5. **What is the difference between `IN` and `EXISTS`?**
   - **`IN`:** Checks if a value is in the result of the subquery.
   - **`EXISTS`:** Checks if the subquery returns any rows (often more efficient for large datasets).

6. **Can a subquery return multiple rows?**
   - Yes, if used with `IN`, `EXISTS`, or as part of a derived table.

7. **What happens if a subquery returns multiple rows where a single row is expected?**
   - SQL throws an error. For instance, this happens if a subquery returns multiple rows in a scalar context (e.g., `= (subquery)`).

8. **How do correlated subqueries impact performance?**
   - Correlated subqueries can be slow because they execute once for every row in the outer query.

9. **What is the difference between a scalar subquery and a table subquery?**
   - **Scalar Subquery:** Returns a single value.
   - **Table Subquery:** Returns a set of rows or columns (used in `FROM`).

10. **Can you use subqueries with aggregate functions?**
    - Yes, you can use subqueries to provide input for aggregate functions.

---

The difference between **`IN`** and **`EXISTS`** with clear examples and scenarios to help you understand.


### **Key Conceptual Differences**

1. **`IN`:**
   - Compares a value with a list of values returned by a subquery or provided directly.
   - Returns `TRUE` if the value is **in the list**.

2. **`EXISTS`:**
   - Tests whether a subquery **returns any rows**.
   - Returns `TRUE` if the subquery returns **at least one row**.



### **Detailed Example**

#### **Scenario: Employee and Department Data**

You have two tables:

1. **`Employees` Table:**
   | **EmployeeID** | **EmployeeName** | **DepartmentID** |
   |----------------|------------------|-------------------|
   | 1              | Alice            | 1                 |
   | 2              | Bob              | 2                 |
   | 3              | Charlie          | 3                 |
   | 4              | David            | NULL              |

2. **`Departments` Table:**
   | **DepartmentID** | **DepartmentName** |
   |------------------|---------------------|
   | 1                | HR                  |
   | 2                | IT                  |



### **Using `IN`**

**Task:** Find employees who work in departments that exist in the `Departments` table.

#### Query:
```sql
SELECT EmployeeName
FROM Employees
WHERE DepartmentID IN (
    SELECT DepartmentID
    FROM Departments
);
```

#### How It Works:
1. The subquery (`SELECT DepartmentID FROM Departments`) returns a list of valid `DepartmentID`s: `[1, 2]`.
2. The outer query checks each employee's `DepartmentID` to see if it matches any value in the list `[1, 2]`.

#### **Result:**
| **EmployeeName** |
|------------------|
| Alice            |
| Bob              |

- Employees in departments `1` and `2` (found in the `Departments` table) are included.
- **Charlie** and **David** are excluded because their `DepartmentID` is not in the list.



### **Using `EXISTS`**

**Task:** Achieve the same result using `EXISTS`.

#### Query:
```sql
SELECT EmployeeName
FROM Employees E
WHERE EXISTS (
    SELECT 1
    FROM Departments D
    WHERE E.DepartmentID = D.DepartmentID
);
```

#### How It Works:
1. For each row in the `Employees` table, the subquery (`SELECT 1 FROM Departments D WHERE E.DepartmentID = D.DepartmentID`) checks if the employee’s `DepartmentID` exists in the `Departments` table.
2. If the subquery finds **at least one match**, `EXISTS` returns `TRUE`, and the employee is included in the result.

#### **Result:**
| **EmployeeName** |
|------------------|
| Alice            |
| Bob              |

- Similar to the `IN` result, employees in departments `1` and `2` are included.



### **Key Difference in How They Work**

#### **1. `IN` Compares Values**
- The subquery in `IN` returns a **list** of values.
- The outer query matches a column (e.g., `DepartmentID`) against this list.

#### **2. `EXISTS` Checks for Row Presence**
- The subquery in `EXISTS` doesn’t return a list. Instead, it simply checks whether any rows exist.
- If the subquery finds even a single row, it evaluates to `TRUE`.


### **Performance Considerations**

#### **When to Use `IN`:**
- Best for a **small list** of values returned by the subquery.
- Example: Matching against a short list of IDs.

#### **When to Use `EXISTS`:**
- Efficient for **large datasets**, especially when:
  - The subquery checks for a condition using `JOINs` or `WHERE`.
  - The goal is to test for the **presence of rows** rather than their values.



### **Edge Case: Null Handling**
1. If the subquery returns `NULL` values:
   - `IN` may produce unexpected results because `NULL` can create ambiguity.
   - `EXISTS` is not affected because it only checks for row presence.

#### **Example with `NULL` Handling**

- Add `DepartmentID = NULL` to the `Departments` table:

| **DepartmentID** | **DepartmentName** |
|------------------|---------------------|
| 1                | HR                  |
| 2                | IT                  |
| NULL             | Undefined           |

- If you run the `IN` query:

```sql
SELECT EmployeeName
FROM Employees
WHERE DepartmentID IN (
    SELECT DepartmentID
    FROM Departments
);
```

Result:
- Alice and Bob are included.
- No match for `Charlie` or `David`.

- **But `NULL` may cause issues in certain scenarios, leading to incorrect results.**

For such cases, `EXISTS` is safer.



### **Common Interview Questions**

1. **What is the difference between `IN` and `EXISTS`?**
   - `IN` compares a value against a list of values.
   - `EXISTS` checks for the presence of rows.

2. **When would you prefer `EXISTS` over `IN`?**
   - When dealing with large datasets or subqueries involving complex joins.
   - When `NULL` handling could cause issues with `IN`.

3. **How does `EXISTS` work internally?**
   - It stops execution as soon as the subquery finds a single row that matches the condition.

4. **What happens if the subquery in `IN` returns `NULL`?**
   - It can cause ambiguity, as `NULL IN (1, NULL)` results in `UNKNOWN`.

---

### **Set Operators in SQL**

Set operators in SQL are used to combine the results of two or more queries. These operators work on the principle of sets (from set theory) and enable comparison or combination of data from multiple `SELECT` statements.


### **Types of Set Operators**

1. **`UNION`**
2. **`UNION ALL`**
3. **`INTERSECT`**
4. **`EXCEPT` (or `MINUS` in some databases)**

---

### **Rules for Using Set Operators**

1. **Same Number of Columns:** Each `SELECT` statement must return the same number of columns.
2. **Compatible Data Types:** The corresponding columns in each `SELECT` must have compatible data types.
3. **Column Order:** The results are matched by column position, not column name.



### **Detailed Explanation with Examples**

#### **1. UNION**
Combines the results of two queries, removing duplicates.

**Syntax:**
```sql
SELECT column1, column2
FROM Table1
UNION
SELECT column1, column2
FROM Table2;
```

**Example:**
| **Table A**            | **Table B**            |
|-------------------------|------------------------|
| 1 (ID)                 | 2 (ID)                |
| 2 (ID)                 | 3 (ID)                |
| 3 (ID)                 | 3 (ID)                |

```sql
SELECT ID FROM TableA
UNION
SELECT ID FROM TableB;
```

**Result:**
| **ID** |
|--------|
| 1      |
| 2      |
| 3      |

- **Duplicates are removed.**



#### **2. UNION ALL**
Combines the results of two queries but **does not remove duplicates**.

**Syntax:**
```sql
SELECT column1, column2
FROM Table1
UNION ALL
SELECT column1, column2
FROM Table2;
```

**Example:**
```sql
SELECT ID FROM TableA
UNION ALL
SELECT ID FROM TableB;
```

**Result:**
| **ID** |
|--------|
| 1      |
| 2      |
| 3      |
| 2      |
| 3      |
| 3      |

- **Duplicates are retained.**



#### **3. INTERSECT**
Returns only the rows that are **common** to both queries.

**Syntax:**
```sql
SELECT column1, column2
FROM Table1
INTERSECT
SELECT column1, column2
FROM Table2;
```

**Example:**
```sql
SELECT ID FROM TableA
INTERSECT
SELECT ID FROM TableB;
```

**Result:**
| **ID** |
|--------|
| 3      |

- **Only the common value (3) is returned.**



#### **4. EXCEPT (or MINUS)**
Returns rows from the first query that are **not present** in the second query.

**Syntax:**
```sql
SELECT column1, column2
FROM Table1
EXCEPT
SELECT column1, column2
FROM Table2;
```

**Example:**
```sql
SELECT ID FROM TableA
EXCEPT
SELECT ID FROM TableB;
```

**Result:**
| **ID** |
|--------|
| 1      |

- **Only rows in TableA and not in TableB are returned.**



### **Comparing Set Operators**

| **Operator**   | **Purpose**                              | **Removes Duplicates?** | **Use Case**                             |
|-----------------|------------------------------------------|--------------------------|------------------------------------------|
| `UNION`        | Combines results from two queries.        | Yes                      | Merging data from two tables without duplicates. |
| `UNION ALL`    | Combines results from two queries.        | No                       | Merging data, allowing duplicates (faster).      |
| `INTERSECT`    | Returns common rows between two queries.  | Yes                      | Finding overlapping data between tables.         |
| `EXCEPT`/`MINUS` | Returns rows in the first query, not in the second. | Yes                      | Finding differences in data.                     |



### **Common Interview Questions**

1. **What are set operators in SQL?**
   - Operators like `UNION`, `UNION ALL`, `INTERSECT`, and `EXCEPT` that combine results from multiple queries.

2. **What is the difference between `UNION` and `UNION ALL`?**
   - `UNION` removes duplicates, while `UNION ALL` retains duplicates.

3. **When would you use `INTERSECT`?**
   - To find rows common to both queries, such as identifying duplicate entries across datasets.

4. **What is the difference between `EXCEPT` and `INTERSECT`?**
   - `EXCEPT` finds rows in the first query not in the second, while `INTERSECT` finds rows common to both.

5. **How does `EXCEPT` handle duplicates?**
   - It removes duplicates from the final result.

6. **Can you use `ORDER BY` with set operators?**
   - Yes, but `ORDER BY` can only appear at the end of the combined query.

7. **What happens if column data types do not match in set operators?**
   - SQL throws an error. Data types must be compatible.



### **Advanced Examples**

#### **1. Combining `UNION` and Aggregates**
Find unique product categories from two tables, sorted by total sales.

```sql
SELECT Category, SUM(Sales)
FROM TableA
GROUP BY Category
UNION
SELECT Category, SUM(Sales)
FROM TableB
GROUP BY Category
ORDER BY SUM(Sales) DESC;
```

#### **2. Using `INTERSECT` for Validation**
Check employees who work in departments available in another table.

```sql
SELECT EmployeeID
FROM Employees
INTERSECT
SELECT DepartmentID
FROM Departments;
```

#### **3. Using `EXCEPT` to Detect Missing Data**
Find customers in TableA who have not made purchases in TableB.

```sql
SELECT CustomerID
FROM TableA
EXCEPT
SELECT CustomerID
FROM TableB;
```

---
