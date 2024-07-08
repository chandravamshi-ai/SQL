# TCL (Transaction Control Language) in SQL

## Table of Contents
1. [Introduction](#introduction)
2. [Beginner Level](#beginner-level)
    - What is TCL?
    - Important TCL Commands
    - Examples
3. [Intermediate Level](#intermediate-level)
    - Transactions
    - Savepoints
    - Examples
4. [Advanced Level](#advanced-level)
    - Isolation Levels
    - Concurrency Control
    - Examples

## Introduction

Transaction Control Language (TCL) is a subset of SQL commands used to manage transactions in a database. A transaction is a sequence of one or more SQL operations that are executed as a single unit of work. The main purpose of TCL is to ensure the integrity and consistency of data by managing the transactions effectively.

## Beginner Level

### What is TCL?

TCL commands are used to control the execution of transactions in a database. They allow you to manage changes made by DML (Data Manipulation Language) statements and ensure that they are committed (saved) or rolled back (undone).

### Important TCL Commands

1. **COMMIT**: Saves the changes made by the current transaction.
2. **ROLLBACK**: Undoes the changes made by the current transaction.
3. **SAVEPOINT**: Sets a savepoint within a transaction to which you can roll back later.

### Examples

Let's consider a simple table named `accounts`:

| account_id | account_name | balance |
|------------|--------------|---------|
| 1          | John Doe     | 1000    |
| 2          | Jane Smith   | 2000    |
| 3          | Alice Jones  | 1500    |

**COMMIT Example:**

1. Start a transaction to update the balance of John Doe:
    ```sql
    BEGIN TRANSACTION;
    UPDATE accounts SET balance = balance - 200 WHERE account_id = 1;
    ```

2. Commit the transaction to save the changes:
    ```sql
    COMMIT;
    ```

After the COMMIT, the balance of John Doe will be updated in the database.

**ROLLBACK Example:**

1. Start a transaction to update the balance of Jane Smith:
    ```sql
    BEGIN TRANSACTION;
    UPDATE accounts SET balance = balance + 500 WHERE account_id = 2;
    ```

2. Rollback the transaction to undo the changes:
    ```sql
    ROLLBACK;
    ```

After the ROLLBACK, the balance of Jane Smith will remain unchanged.

## Intermediate Level

### Transactions

A transaction is a logical unit of work that contains one or more SQL statements. Transactions follow the ACID properties:
- **Atomicity**: Ensures that all operations within the transaction are completed successfully. If not, the transaction is aborted.
- **Consistency**: Ensures the database remains in a consistent state before and after the transaction.
- **Isolation**: Ensures that transactions are executed in isolation from other transactions.
- **Durability**: Ensures that the changes made by a committed transaction are permanent.

### Savepoints

A savepoint is a point within a transaction to which you can roll back without affecting the entire transaction. This allows for finer control over transaction management.

### Examples

**SAVEPOINT Example:**

1. Start a transaction to update multiple balances:
    ```sql
    BEGIN TRANSACTION;
    UPDATE accounts SET balance = balance - 100 WHERE account_id = 1;
    SAVEPOINT savepoint1;
    UPDATE accounts SET balance = balance + 100 WHERE account_id = 2;
    ```

2. If you decide to undo the second update but keep the first, you can roll back to the savepoint:
    ```sql
    ROLLBACK TO savepoint1;
    ```

3. Commit the transaction to save the changes up to the savepoint:
    ```sql
    COMMIT;
    ```

After these operations, only the balance of John Doe will be updated.

## Advanced Level

### Isolation Levels

Isolation levels define the degree to which the operations in one transaction are isolated from those in other transactions. SQL defines four isolation levels:
1. **Read Uncommitted**: Allows transactions to read data that is being modified by other transactions.
2. **Read Committed**: Ensures that a transaction can only read data that has been committed.
3. **Repeatable Read**: Ensures that if a transaction reads data, it will get the same data if it reads it again during the same transaction.
4. **Serializable**: Ensures complete isolation from other transactions.

### Concurrency Control

Concurrency control is the management of simultaneous transaction execution in a multi-user database system. Proper concurrency control ensures data consistency and integrity while allowing multiple transactions to occur simultaneously.

### Examples

**Isolation Levels Example:**

Consider two transactions:

- **Transaction A**: Reads and updates the balance of John Doe.
- **Transaction B**: Reads the balance of John Doe.

**Read Committed Example:**

1. Transaction A starts and updates the balance:
    ```sql
    BEGIN TRANSACTION;
    UPDATE accounts SET balance = balance - 200 WHERE account_id = 1;
    ```

2. Transaction B tries to read the balance before Transaction A commits:
    ```sql
    SET TRANSACTION ISOLATION LEVEL READ COMMITTED;
    SELECT balance FROM accounts WHERE account_id = 1;
    ```

Transaction B will not see the uncommitted balance change made by Transaction A.

**Serializable Example:**

1. Transaction A starts and reads the balance:
    ```sql
    BEGIN TRANSACTION;
    SET TRANSACTION ISOLATION LEVEL SERIALIZABLE;
    SELECT balance FROM accounts WHERE account_id = 1;
    ```

2. Transaction B tries to update the balance before Transaction A completes:
    ```sql
    BEGIN TRANSACTION;
    UPDATE accounts SET balance = balance - 200 WHERE account_id = 1;
    ```

Transaction B will be blocked until Transaction A is completed to ensure complete isolation.

## Conclusion

Understanding and effectively using TCL commands in SQL is crucial for maintaining the integrity and consistency of your database. By mastering these commands at different levels, you can manage transactions efficiently and ensure your database operations are reliable and secure.
