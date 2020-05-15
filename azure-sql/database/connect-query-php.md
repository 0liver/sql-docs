---
title: Use PHP to query
description: How to use PHP to create a program that connects to an Azure SQL Database and query it using T-SQL statements.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.devlang: php
ms.topic: quickstart
author: stevestein
ms.author: sstein
ms.reviewer: v-masebo
ms.date: 02/12/2019
ms.custom: sqldbrb=2 
---
# Quickstart: Use PHP to query an Azure SQL Database

This article demonstrates how to use [PHP](https://php.net/manual/en/intro-whatis.php) to connect to an Azure SQL Database. You can then use T-SQL statements to query data.

## Prerequisites

To complete this sample, make sure you have the following prerequisites:

- An Azure SQL Database. You can use one of these quickstarts to create and then configure a database in Azure SQL Database:

  || SQL Database | SQL Managed Instance |
  |:--- |:--- |:---|
  | Create| [Portal](single-database-create-quickstart.md) | [Portal](../managed-instance/instance-create-quickstart.md) |
  || [CLI](scripts/create-and-configure-database-cli.md) | [CLI](https://medium.com/azure-sqldb-managed-instance/working-with-sql-managed-instance-using-azure-cli-611795fe0b44) |
  || [PowerShell](scripts/create-and-configure-database-powershell.md) | [PowerShell](../managed-instance/scripts/create-configure-managed-instance-powershell.md) |
  | Configure | [Server-level IP firewall rule](firewall-create-server-level-portal-quickstart.md)| [Connectivity from a VM](../managed-instance/connect-vm-instance-configure.md)|
  |||[Connectivity from on-site](../managed-instance/point-to-site-p2s-configure.md)
  |Load data|Adventure Works loaded per quickstart|[Restore Wide World Importers](../managed-instance/restore-sample-database-quickstart.md)
  |||Restore or import Adventure Works from [BACPAC](database-import.md) file from [GitHub](https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/adventure-works)|
  |||

  > [!IMPORTANT]
  > The scripts in this article are written to use the Adventure Works database. With a managed instance, you must either import the Adventure Works database into an instance database or modify the scripts in this article to use the Wide World Importers database.

- PHP-related software installed for your operating system:

  - **MacOS**, install PHP, the ODBC driver, then install the PHP Driver for SQL Server. See [Step 1, 2, and 3](/sql/connect/php/installation-tutorial-linux-mac).

  - **Linux**, install PHP, the ODBC driver, then install the PHP Driver for SQL Server. See [Step 1, 2, and 3](/sql/connect/php/installation-tutorial-linux-mac).

  - **Windows**, install PHP for IIS Express and Chocolatey, then install the ODBC driver and SQLCMD. See [Step 1.2 and 1.3](https://www.microsoft.com/sql-server/developer-get-started/php/windows/).

## Get SQL server connection information

Get the connection information you need to connect to the Azure SQL Database. You'll need the fully qualified server name or host name, database name, and login information for the upcoming procedures.

1. Sign in to the [Azure portal](https://portal.azure.com/).

2. Navigate to the **SQL Databases**  or **SQL Managed Instances** page.

3. On the **Overview** page, review the fully qualified server name next to **Server name** for an Azure SQL Database or the fully qualified server name next to **Host** for an Azure SQL Managed Instance. To copy the server name or host name, hover over it and select the **Copy** icon.

## Add code to query database

1. In your favorite text editor, create a new file, *sqltest.php*.  

1. Replace its contents with the following code. Then add the appropriate values for your server, database, user, and password.

   ```PHP
   <?php
       $serverName = "your_server.database.windows.net"; // update me
       $connectionOptions = array(
           "Database" => "your_database", // update me
           "Uid" => "your_username", // update me
           "PWD" => "your_password" // update me
       );
       //Establishes the connection
       $conn = sqlsrv_connect($serverName, $connectionOptions);
       $tsql= "SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName
            FROM [SalesLT].[ProductCategory] pc
            JOIN [SalesLT].[Product] p
            ON pc.productcategoryid = p.productcategoryid";
       $getResults= sqlsrv_query($conn, $tsql);
       echo ("Reading data from table" . PHP_EOL);
       if ($getResults == FALSE)
           echo (sqlsrv_errors());
       while ($row = sqlsrv_fetch_array($getResults, SQLSRV_FETCH_ASSOC)) {
        echo ($row['CategoryName'] . " " . $row['ProductName'] . PHP_EOL);
       }
       sqlsrv_free_stmt($getResults);
   ?>
   ```

## Run the code

1. At the command prompt, run the app.

   ```bash
   php sqltest.php
   ```

1. Verify the top 20 rows are returned and close the app window.

## Next steps

- [Design your first Azure SQL Database](design-first-database-tutorial.md)

- [Microsoft PHP Drivers for SQL Server](https://github.com/Microsoft/msphpsql/)

- [Report issues or ask questions](https://github.com/Microsoft/msphpsql/issues)

- [Retry logic example: Connect resiliently to SQL with PHP](/sql/connect/php/step-4-connect-resiliently-to-sql-with-php)
