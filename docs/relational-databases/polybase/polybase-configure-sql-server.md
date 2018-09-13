---
title: "Configure PolyBase to access external data in SQL Server | Microsoft Docs"
ms.custom: ""
ms.date: "09/24/2018"
ms.prod: sql
ms.reviewer: ""
ms.suite: "sql"
ms.technology: polybase
ms.tgt_pltfrm: ""
ms.topic: conceptual
author: Abiola
ms.author: aboke
manager: craigg
monikerRange: ">= sql-server-ver15 || = sqlallproducts-allversions"
---
# Configure PolyBase to access external data in SQL Server

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

The article explains how to use PolyBase on a SQL Server instance to query external data in another SQL Server instance.

## Prerequisites

If you haven't installed PolyBase, see [PolyBase installation](polybase-installation.md). The installation article explains the prerequisites.

## Configure an External Table

To query the data from a SQL Server data source, you must create external tables to reference the external data. This section provides sample code to create these external tables. 
 
We recommend creating statistics on external table columns, especially the ones used for joins, filters and aggregates, for optimal query performance.

Here are the objects we will create in this section:

- CREATE DATABASE SCOPED CREDENTIAL (Transact-SQL) 
- CREATE EXTERNAL DATA SOURCE (Transact-SQL) 
- CREATE EXTERNAL FILE FORMAT (Transact-SQL) 
- CREATE EXTERNAL TABLE (Transact-SQL) 
- CREATE STATISTICS (Transact-SQL)
.

1. Create a master key on the database. This is required to encrypt the credential secret.
  ```sql
   CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'S0me!nfo';  
   ```

2. Create a database scoped credential for.

 ```sql
 /*  specify credentials to external data source
 *  IDENTITY: user name for external source.  
 *  SECRET: password for external source.
 */
CREATE DATABASE SCOPED CREDENTIAL SqlServerCredentials   
WITH IDENTITY = 'username', Secret = 'password';
  ```

3. Create an external data source with [CREATE EXTERNAL DATA SOURCE](../../t-sql/statements/create-external-data-source-transact-sql.md).Specify external data source location and credentials for SQL Server.

  ```sql
 /*  LOCATION: Server DNS name or IP address.
 *  PUSHDOWN: specify whether computation should be pushed down to the source. ON by default.
 *  CREDENTIAL: the database scoped credential, created above.
 */  
CREATE EXTERNAL DATA SOURCE SqlServerInstance
WITH ( 
  LOCATION = 'sqlserver://TestDBs',
  -- PUSHDOWN = ON | OFF,
  CREDENTIAL = SqlServerCredentials
);

   ```

4. Create an external file format with [CREATE EXTERNAL FILE FORMAT](../../t-sql/statements/create-external-file-format-transact-sql.md).

  ```sql
/*  specify external file format
 *  FORMAT TYPE: Type of format in Hadoop - DELIMITEDTEXT, RCFILE, ORC, PARQUET
 *  for compressed Parquet files, specify DATA_COMPRESSION  
 */
CREATE EXTERNAL FILE FORMAT Parquet
WITH (
    FORMAT_TYPE = PARQUET
    -- , DATA_COMPRESSION = 'org.apache.hadoop.io.compress.SnappyCodec'
); 
   ```

5. Create schemas for external data

  ```sql
CREATE SCHEMA sqlserver;
GO
 
   ```
6.  Create external tables that represents data stored in external SQL Server  [CREATE EXTERNAL TABLE](../../t-sql/statements/create-external-table-transact-sql.md).
 
 ```sql
/*  LOCATION: sql server table/view in 'database_name.schema_name.object_name' format
 *  DATA_SOURCE: the external data source, created above.
 */
CREATE EXTERNAL TABLE sqlserver.customer(
    C_CUSTKEY INT NOT NULL,
    C_NAME VARCHAR(25) NOT NULL,
    C_ADDRESS VARCHAR(40) NOT NULL,
    C_NATIONKEY INT NOT NULL,
    C_PHONE CHAR(15) NOT NULL,
    C_ACCTBAL DECIMAL(15,2) NOT NULL,
    C_MKTSEGMENT CHAR(10) NOT NULL,
    C_COMMENT VARCHAR(117) NOT NULL
)
WITH (
    LOCATION='tpch_10.dbo.customer',
    DATA_SOURCE=SqlServerInstance
);

```
7. Create statistics on an external table.

  ```sql
    CREATE STATISTICS CustomerCustKeyStatistics ON sqlserver.customer(C_CUSTKEY) WITH FULLSCAN; 
   ```



## Next steps

To learn more about PolyBase, see [Overview of SQL Server PolyBase](polybase-guide.md).