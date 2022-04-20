---
description: "sys.sp_copy_data_in_batches (Transact-SQL)"
title: "sys.sp_copy_data_in_batches (Transact-SQL) | Microsoft Docs"
ms.custom: ""
ms.date: "05/25/2021"
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

# sys.sp_copy_data_in_batches (Transact-SQL)

[!INCLUDE [Azure SQL Database](../../includes/applies-to-version/asdb.md)]

Copies data from the source table to the target table after verifying that their schema is identical in terms of number of columns, column names and their data types. TRANSACTION ID, SEQUENCE NUMBER and  GENERATED ALWAYS columns are ignored since they are system generated and this allows copying data from a regular table to a ledger table and vice versa. Indexes between the tables can be different but the target table can only be a Heap or have a clustered index. The data is copied in batches in individual transactions. If the operation fails, the target table will be partially populated. 

For more information on database ledger, see [Azure SQL Database ledger](/azure/azure-sql/database/ledger-overview)

![Topic link icon](../../database-engine/configure-windows/media/topic-link.gif "Topic link icon") [Transact-SQL Syntax Conventions](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md) 

## Syntax  
  
```syntaxsql
sp_copy_data_in_batches [@source_table_name = ] N'source_table_name' , [@targe_table_name = ] N'target_table_name'
```

## Arguments

[ @source_table_name = ] N'source_table_name'
The name of the table to be used as the source of the data copy.

[ @target_table_name = ] N'target_table_name'
The name of the table to be used as the target of the data copy.

## Return code values

0 (success)

## Result sets

None.

## Permissions

This operation requires **SELECT** on the source table, **INSERT** in the target table and **ALTER** on the target table if there are FK or Check constraints that will be disabled or an Identity column that will be adjusted. 

## See also

- [Ledger considerations and limitations](/docs/relational-databases/security/ledger/ledger-limits.md)
- [Ledger Overview](/docs/relational-databases/security/ledger/ledger-overview.md)
- [How to convert regular tables into ledger tables](ledger-how-to-convert-regular-tables-into-ledger-tables.md)
