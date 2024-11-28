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
