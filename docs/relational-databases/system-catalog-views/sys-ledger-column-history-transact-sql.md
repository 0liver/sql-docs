---
description: "sys.ledger_column_history (Transact-SQL)"
title: "sys.ledger_column_history (Transact-SQL) | Microsoft Docs"
ms.custom: ""
ms.date: "03/22/2022"
ms.prod: sql
ms.reviewer: ""
ms.technology: system-objects
ms.topic: "reference"
dev_langs: 
  - "TSQL"
author: VanMSFT
ms.author: vanto
monikerRange: "=azuresqldb-current"
---
# sys.ledger_column_history (Transact-SQL)

[!INCLUDE [Azure SQL Database](../../includes/applies-to-version/asdb.md)]

Captures the cryptographically protected history of operations on columns of ledger tables: adding, renaming, and dropping columns.

For more information on database ledger, see [Azure SQL Database ledger](/azure/azure-sql/database/ledger-overview)

|Column name|Data type|Description|  
|-----------------|---------------|-----------------|
|**object_id**|**int**|The object ID of the ledger table.|
|**column_id**|**int**|The column ID of the column in a ledger table. |
|**column_name**|**sysname**|The name of the column in ledger table. If the operation has changed the column name, this column captures the new column name.|
|**operation_type**|**tinyint**|The numeric value indicating the type of the operation<br/><br/>0 = CREATE – creating a column as part of creating the table containing the column using CREATE TABLE.<br/>1 = ADD – adding a column in a ledger table, using ALTER TABLE/ADD COLUMN. <br/> 2 = RENAME - renaming a column in a ledger table.<br/>3 = DROP - dropping a column in a ledger table.|
|**operation_type_desc**|**nvarchar(60)**|Textual description of the value of operation_type.|
|**transaction_id**| **bigint** | A transaction ID that is unique for the database (it corresponds to a transaction ID in the database transaction log). |
|**sequence_number**| **bigint** | The sequence number of the operation within the transaction. |

## Permissions

Requires the **VIEW LEDGER CONTENT** permission.

## Examples

Consider the following sequence of operations on ledger tables.

1. A user creates a ledger table.

    ```sql
    CREATE TABLE [Employees]
    (
        EmployeeID INT NOT NULL,
        Salary Money NOT NULL
    )
    WITH (SYSTEM_VERSIONING = ON, LEDGER = ON);
    GO
    ```

1. A user adds a column to the ledger table.

    ```sql
    ALTER TABLE [Employees] ADD Lastname NVARCHAR(256) NULL;
    ```

1. A user renames a column of the ledger table.

    ```sql
    EXEC sp_rename 'dbo.Employees.Lastname', 'Firstname', 'COLUMN';
    ```

1. A user drops a column of the ledger table.

    ```sql
    ALTER TABLE [Employees] DROP COLUMN Firstname;
    ```

The below query joins sys.ledger_column_history and sys.database_ledger_transactions to produce the history of changes on ledger table columns, including the time of each and change and the name of the user who triggered it.

```sql
SELECT 
t.[principal_name]
, t.[commit_time]
, h.[column_name] AS [column_name]
, h.[operation_type_desc]
FROM sys.ledger_column_history h
JOIN sys.database_ledger_transactions t
ON h.transaction_id = t.transaction_id
ORDER BY t.[commit_time];
```

## See also

- [Ledger considerations and limitations](/docs/relational-databases/security/ledger/ledger-limits.md)
- [Ledger Overview](/docs/relational-databases/security/ledger/ledger-overview.md)