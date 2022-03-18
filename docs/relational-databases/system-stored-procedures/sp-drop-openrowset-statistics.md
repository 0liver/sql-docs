---
description: "The sp_drop_openrowset_statistics system stored procedure removes column statistics for a column in the OPENROWSET path of Azure Synapse SQL resources."
title: "sp_drop_openrowset_statistics (Transact-SQL)"
ms.custom: ""
ms.date: "03/11/2022"
ms.prod: sql
ms.prod_service: "database-engine, sql-database, synapse-analytics"
ms.reviewer: ""
ms.technology: system-objects
ms.topic: "reference"
f1_keywords: 
  - "sp_drop_openrowset_statistics_TSQL"
  - "sp_drop_openrowset_statistics"
dev_langs: 
  - "TSQL"
helpviewer_keywords: 
  - "sp_drop_openrowset_statistics"
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: "=azure-sqldw-latest||=azuresqldb-mi-current"
---
# sp_drop_openrowset_statistics (Transact-SQL)
[!INCLUDE [asdbmi-asa-svrless-poolonly](../../includes/applies-to-version/asdbmi-asa-svrless-poolonly.md)]

  Drops column statistics for a column in the OPENROWSET path of Azure Synapse SQL resources, such as dedicated SQL pools and serverless SQL pools. For more information, see [Statistics in Synapse SQL](/azure/synapse-analytics/sql/develop-tables-statistics). To update statistics, drop and create statistics using [sp_create_openrowset_statistics (Transact-SQL)](sp-create-openrowset-statistics.md).

  This procedure is also used by [!INCLUDE[ssazuremi_md](../../includes/ssazuremi_md.md)] for column statistics in external data sources via OPENROWSET.

 ![Topic link icon](../../database-engine/configure-windows/media/topic-link.gif "Topic link icon") [Transact-SQL Syntax Conventions](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## Syntax  
  
```syntaxsql  
sys.sp_drop_openrowset_statistics [ @stmt = ] N'statement_text'
```  

## Arguments  

[ @stmt = ] N'statement_text'
Specifies a Transact-SQL statement that will return column values to be used for statistics. You can use TABLESAMPLE within the `@stmt` to specify samples of data to be used. If TABLESAMPLE isn't specified, FULLSCAN will be used.

`<tablesample_clause> ::= TABLESAMPLE ( sample_number PERCENT )`
  
## Remarks  
 Statistics metadata is not available for OPENROWSET columns.

## Permissions  
 Requires ADMINISTER BULK OPERATIONS or ADMINISTER DATABASE BULK OPERATIONS permissions.
  
## Example

For scenarios and examples, review [Update statistics](/azure/synapse-analytics/sql/develop-tables-statistics#examples-update-statistics-1).
  
## See also  

- [sys.sp_create_openrowset_statistics (Transact-SQL)](sp-create-openrowset-statistics.md)
 
## Next steps

- [Statistics in Synapse SQL](/azure/synapse-analytics/sql/develop-tables-statistics)