---
title: "Oracle CDC Instance Data Types"
description: "Oracle CDC Instance Data Types"
author: chugugrace
ms.author: chugu
ms.date: "03/01/2017"
ms.service: sql
ms.subservice: integration-services
ms.topic: conceptual
---
# Oracle CDC Instance Data Types

[!INCLUDE [oracle-cdc-retirement](../includes/attunity-oracle-cdc-retirement.md)]

  The Oracle CDC Instance supports most Oracle data types. The following sections describe the supported data types and the non-supported data types.  
  
## Supported Data Types  
 The following table describes the Oracle data types that can be captured and their default mapping to SQL Server data types in the change tables. When adding a capture instance for a source Oracle table, you can override some of these mappings.  
  
|Oracle Database Data Type|SQL Server Data Type|  
|-------------------------------|--------------------------|  
|BINARY_FLOAT|REAL|  
|BINARY_DOUBLE|FLOAT|  
|CHAR|NVARCHAR|  
|DATE|DATETIME|  
|FLOAT|FLOAT|  
|INT|NUMERIC (38)|  
|INTERVAL|DATETIME|  
|NCHAR|NVARCHAR|  
|NUMBER|FLOAT|  
|NAVARCHAR2|NVARCHAR|  
|RAW|VARBINARY|  
|REAL|FLOAT|  
|TIMESTAMP|DATETIME2|  
|TIMESTAMP WITH TIME ZONE|VARCHAR (37)|  
|TIMESTAMP WITH LOCAL TIME ZONE|VARCHAR (37)|  
|VARCHAR2|VARCHAR|  
|XMLTYPE|NVARCHAR (MAX)|  
  
## Non-Supported Data Types  
 Source Oracle tables with columns of the following Oracle data types cannot be captured. Captured columns with these data types will show as null; however, a change in their value is indicated in the change mask of the captured tables.  
  
-   LONG  
  
-   XMLTYPE  
  
-   BLOB  
  
-   CLOB  
  
 Source Oracle tables with columns of the following Oracle data types cannot be captured.  
  
-   BFILE  
  
-   ROWID  
  
-   REF  
  
-   UROWID  
  
-   Nested Table  
  
 If the following data types are present in a table they will prevent the LogMinder from getting any data for any column of the table:  
  
-   User-defined data types  
  
-   VARRAY  
  
## See Also  
 [Change Data Capture Designer for Oracle by Attunity](../../integration-services/change-data-capture/change-data-capture-designer-for-oracle-by-attunity.md)   
 [The Oracle CDC Instance](../../integration-services/change-data-capture/the-oracle-cdc-instance.md)  
  
  
