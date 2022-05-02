---
title: "Modify XML indexes"
description: Learn how the ALTER INDEX (Transact-SQL) DDL statement can be used to modify existing XML and non-XML indexes.
ms.custom: ""
ms.date: 04/29/2022
ms.prod: sql
ms.prod_service: "database-engine"
ms.reviewer: randolphwest
ms.technology: xml
ms.topic: conceptual
helpviewer_keywords:
  - "XML indexes [SQL Server], modifying"
  - "modifying indexes"
ms.assetid: 24d50fe1-c6ec-49e6-91a3-9791851ba53d
author: MikeRayMSFT
ms.author: mikeray
---
# Modify XML indexes

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

The [ALTER INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-index-transact-sql.md)[!INCLUDE[tsql](../../includes/tsql-md.md)] DDL statement can be used to modify existing XML and non-XML indexes. However, not all the ALTER INDEX options are available to XML indexes. The following options are not valid when modifying XML indexes:

- The rebuild and set option IGNORE_DUP_KEY is not valid for XML indexes. The rebuild option ONLINE must be set to OFF for secondary XML indexes. The option DROP_EXISTING is not permitted in the ALTER INDEX statement.

- The modifications of the primary key constraint in the user table are not automatically propagated to XML indexes. The user must drop the XML indexes first and then re-create them.

- If ALTER INDEX ALL is specified, it applies to both non-XML and XML indexes. Indexing options may be specified that are not valid for both types of indexes. In this case, the whole statement fails.

## Examples

### Modify an XML index

 In the following example, an XML index is created and then modified by setting the option `ALLOW_ROW_LOCKS` to `OFF`. When `ALLOW_ROW_LOCKS` is `OFF`, rows are not locked and access to the specified indexes is obtained by using page-and table-level locks.

```sql
CREATE TABLE T (Col1 INT PRIMARY KEY, XmlCol XML);
GO
-- Create primary XML index.
CREATE PRIMARY XML INDEX PIdx_T_XmlCol
ON T(XmlCol);
GO
-- Note the type 3 is index on XML type.
SELECT *
FROM sys.xml_indexes
WHERE object_id = object_id('T')
AND name='PIdx_T_XmlCol';

-- Modify and set an index option.
ALTER INDEX PIdx_T_XmlCol on T
SET (ALLOW_ROW_LOCKS = OFF);
```

### Disable and enable an XML index

By default, an XML index is enabled. If an XML index is disabled, the queries running against the XML column do not use the XML index. To enable an XML index, use `ALTER INDEX` with the `REBUILD` option.

```sql
CREATE TABLE T (Col1 INT PRIMARY KEY, XmlCol XML);
GO
CREATE PRIMARY XML INDEX PIdx_T_XmlCol ON T(XmlCol);
GO
ALTER INDEX PIdx_T_XmlCol on T DISABLE;
GO
```

Use the following sample to verify the index is disabled.

```sql
SELECT *
FROM sys.xml_indexes
WHERE object_id = object_id('T')
AND name='PIdx_T_XmlCol';
GO
```

Use the following sample to rebuild the index.

```sql
ALTER INDEX PIdx_T_XmlCol on T REBUILD;
GO
```

## See also

- [XML indexes &#40;SQL Server&#41;](../../relational-databases/xml/xml-indexes-sql-server.md)
