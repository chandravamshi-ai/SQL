# SQL
SQL Notes

---
## Small Intro to SQL

SQL is divided into different categories or components, each serving specific purposes. Here are the main components of SQL:

### 1. **DDL (Data Definition Language)**

DDL commands are used to define and manage database structures such as tables, indexes, and views. The main DDL commands are:

#### a. **CREATE**
- **Purpose**: Create a new database object (e.g., table, index).
- **Example**:
  ```sql
  CREATE TABLE students (
    id INT PRIMARY KEY,
    name VARCHAR(50),
    age INT,
    grade VARCHAR(10)
  );
  ```

#### b. **ALTER**
- **Purpose**: Modify an existing database object.
- **Example**:
  ```sql
  ALTER TABLE students ADD email VARCHAR(50);
  ```

#### c. **DROP**
- **Purpose**: Delete a database object.
- **Example**:
  ```sql
  DROP TABLE students;
  ```

#### d. **TRUNCATE**
- **Purpose**: Remove all rows from a table without deleting the table structure.
- **Example**:
  ```sql
  TRUNCATE TABLE students;
  ```

### 2. **DML (Data Manipulation Language)**

DML commands are used to manage data within the database objects. The main DML commands are:

#### a. **INSERT**
- **Purpose**: Add new data to a table.
- **Example**:
  ```sql
  INSERT INTO students (name, age, grade) VALUES ('John Doe', 15, '10th');
  ```

#### b. **UPDATE**
- **Purpose**: Modify existing data in a table.
- **Example**:
  ```sql
  UPDATE students SET grade = '11th' WHERE name = 'John Doe';
  ```

#### c. **DELETE**
- **Purpose**: Remove data from a table.
- **Example**:
  ```sql
  DELETE FROM students WHERE name = 'John Doe';
  ```

### 3. **DQL (Data Query Language)**

DQL is used to query the database and retrieve data. The primary DQL command is:

#### a. **SELECT**
- **Purpose**: Retrieve data from a table.
- **Example**:
  ```sql
  SELECT name, age FROM students;
  ```

### 4. **DCL (Data Control Language)**

DCL commands are used to control access to data in the database. The main DCL commands are:

#### a. **GRANT**
- **Purpose**: Give user access privileges to the database.
- **Example**:
  ```sql
  GRANT SELECT, INSERT ON students TO user_name;
  ```

#### b. **REVOKE**
- **Purpose**: Remove user access privileges.
- **Example**:
  ```sql
  REVOKE SELECT, INSERT ON students FROM user_name;
  ```

### 5. **TCL (Transaction Control Language)**

TCL commands are used to manage transactions in the database. The main TCL commands are:

#### a. **COMMIT**
- **Purpose**: Save the changes made in a transaction.
- **Example**:
  ```sql
  COMMIT;
  ```

#### b. **ROLLBACK**
- **Purpose**: Undo changes made in the current transaction.
- **Example**:
  ```sql
  ROLLBACK;
  ```

#### c. **SAVEPOINT**
- **Purpose**: Set a point within a transaction to which you can later roll back.
- **Example**:
  ```sql
  SAVEPOINT savepoint_name;
  ```

#### d. **SET TRANSACTION**
- **Purpose**: Specify characteristics for the current transaction.
- **Example**:
  ```sql
  SET TRANSACTION READ ONLY;
  ```

### Summary of SQL Components

- **DDL (Data Definition Language)**: Deals with database structure. Commands: `CREATE`, `ALTER`, `DROP`, `TRUNCATE`.
- **DML (Data Manipulation Language)**: Deals with data manipulation. Commands: `INSERT`, `UPDATE`, `DELETE`.
- **DQL (Data Query Language)**: Deals with querying data. Command: `SELECT`.
- **DCL (Data Control Language)**: Deals with permissions and access control. Commands: `GRANT`, `REVOKE`.
- **TCL (Transaction Control Language)**: Deals with transaction management. Commands: `COMMIT`, `ROLLBACK`, `SAVEPOINT`, `SET TRANSACTION`.

Each of these components plays a crucial role in managing and interacting with the database.
