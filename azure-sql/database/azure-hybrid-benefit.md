---
title: Azure Hybrid Benefit 
titleSuffix: Azure SQL Database & SQL Managed Instance 
description: Use existing SQL Server licenses for Azure SQL Database and SQL Managed Instance discounts.
services: sql-database
ms.service: sql-database
ms.custom: sqldbrb=4
ms.subservice: service
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: sashan, moslake, carlrab
ms.date: 11/13/2019
---
# Azure Hybrid Benefit - Azure SQL Database & SQL Managed Instance

In the provisioned compute tier of the vCore-based purchasing model, you can exchange your existing licenses for discounted rates on Azure SQL Database and Azure SQL Managed Instance by using [Azure Hybrid Benefit](https://azure.microsoft.com/pricing/hybrid-benefit/). This Azure benefit allows you to save up to 30 percent or even higher on SQL Database & SQL Managed Instance by using your on-premises SQL Server licenses with Software Assurance. The [Azure Hybrid Benefit](https://azure.microsoft.com/pricing/hybrid-benefit/) page has a calculator to help determine savings.

> [!NOTE]
> Changing to Azure Hybrid Benefit does not require any downtime.

![pricing](./media/azure-hybrid-benefit/pricing.png)

## Choose a license model

With Azure Hybrid Benefit, you can choose to pay only for the underlying Azure infrastructure by using your existing SQL Server license for the SQL database engine itself (Base Compute pricing), or you can pay for both the underlying infrastructure and the SQL Server license (License-Included pricing).

You can choose or change your licensing model by using the Azure portal or by using one of the following APIs:

# [PowerShell](#tab/azure-powershell)

To set or update the license type by using PowerShell:

- [New-AzSqlDatabase](/powershell/module/az.sql/new-azsqldatabase)
- [Set-AzSqlDatabase](/powershell/module/az.sql/set-azsqldatabase)
- [New-AzSqlInstance](/powershell/module/az.sql/new-azsqlinstance)
- [Set-AzSqlInstance](/powershell/module/az.sql/set-azsqlinstance)

# [Azure CLI](#tab/azure-cli)

To set or update the license type by using the Azure CLI:

- [az sql db create](/cli/azure/sql/db#az-sql-db-create)
- [az sql db update](/cli/azure/sql/db#az-sql-db-update)
- [az sql mi create](/cli/azure/sql/mi#az-sql-mi-create)
- [az sql mi update](/cli/azure/sql/mi#az-sql-mi-update)

# [REST API](#tab/rest)

To set or update the license type by using the REST API:

- [Databases - Create Or Update](/rest/api/sql/databases/createorupdate)
- [Databases - Update](/rest/api/sql/databases/update)
- [Managed Instances - Create Or Update](/rest/api/sql/managedinstances/createorupdate)
- [Managed Instances - Update](/rest/api/sql/managedinstances/update)

* * *

## Next steps

- For for help with choosing an Azure SQL deployment option, see [Choose the right deployment option in Azure SQL](../paas-vs-sql-server-iaas.md).
- For a comparison of SQL Database and SQL Managed Instance features, see [SQL Database & SQL Managed Instance features](../../sql-database/sql-database-features.md).
