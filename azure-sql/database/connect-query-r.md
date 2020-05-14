---
title: Use R with Machine Learning Services to query a database (preview)
titleSuffix: Azure SQL Database Machine Learning Services (preview)
description: This article shows you how to use an R script with Azure SQL Database Machine Learning Services to connect to an Azure SQL Database and query it using Transact-SQL statements.
services: sql-database
ms.service: sql-database
ms.subservice: machine-learning
ms.custom: sqldbrb=2 
ms.devlang: python
ms.topic: quickstart
author: garyericson
ms.author: garye
ms.reviewer: davidph, carlrab
manager: cgronlun
ms.date: 05/29/2019
ROBOTS: NOINDEX
---

# Quickstart: Use R with Machine Learning Services to query an Azure SQL Database (preview)

In this quickstart, you use R with Machine Learning Services to connect to an Azure SQL Database and use T-SQL statements to query data.

[!INCLUDE[ml-preview-note](../../../includes/sql-database-ml-preview-note.md)]

## Prerequisites

- An Azure account with an active subscription. [Create an account for free](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio).
- An [Azure SQL Database](../../sql-database/sql-database-single-database-get-started.md)
- [Machine Learning Services](machine-learning-services-overview.md) with R enabled.
- [SQL Server Management Studio](/sql/ssms/sql-server-management-studio-ssms) (SSMS)

> [!IMPORTANT]
> The scripts in this article are written to use the **Adventure Works** database.

Machine Learning Services with R is a feature of Azure SQL Database used for executing in-database R scripts. For more information, see the [R Project](https://www.r-project.org/).

## Get SQL server connection information

Get the connection information you need to connect to the Azure SQL Database. You'll need the fully qualified server name or host name, database name, and login information for the upcoming procedures.

1. Sign in to the [Azure portal](https://portal.azure.com/).

2. Navigate to the **SQL Databases**  or **SQL Managed Instances** page.

3. On the **Overview** page, review the fully qualified server name next to **Server name** for an Azure SQL Database or the fully qualified server name next to **Host** for an Azure SQL Managed Instance. To copy the server name or host name, hover over it and select the **Copy** icon.

## Create code to query your SQL Database

1. Open **SQL Server Management Studio** and connect to your SQL Database.

   If you need help connecting, see [Quickstart: Use SQL Server Management Studio to connect and query an Azure SQL Database](connect-query-ssms.md).

1. Pass the complete R script to the [sp_execute_external_script](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql) stored procedure.

   The script is passed through the `@script` argument. Everything inside the `@script` argument must be valid R code.
   
   >[!IMPORTANT]
   >The code in this example uses the sample AdventureWorksLT data, which you can choose as source when creating your database. If your database has different data, use tables from your own database in the SELECT query. 

    ```sql
    EXECUTE sp_execute_external_script
    @language = N'R'
    , @script = N'OutputDataSet <- InputDataSet;'
    , @input_data_1 = N'SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName FROM [SalesLT].[ProductCategory] pc JOIN [SalesLT].[Product] p ON pc.productcategoryid = p.productcategoryid'
    ```

   > [!NOTE]
   > If you get any errors, it might be because the public preview of Machine Learning Services (with R) is not enabled for your SQL Database. See [Prerequisites](#prerequisites) above.

## Run the code

1. Execute the [sp_execute_external_script](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql) stored procedure.

1. Verify that the top 20 Category/Product rows are returned in the **Messages** window.

## Next steps

- [Design your first Azure SQL Database](../../sql-database/sql-database-design-first-database.md)
- [Azure SQL Database Machine Learning Services (with R)](machine-learning-services-overview.md)
- [Create and run simple R scripts in Azure SQL Database Machine Learning Services (preview)](quickstart-r-create-script.md)
- [Write advanced R functions in Azure SQL Database using Machine Learning Services (preview)](machine-learning-services-functions.md)
