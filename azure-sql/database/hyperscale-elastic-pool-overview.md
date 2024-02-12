---
title: Hyperscale elastic pools overview
description: Manage and scale multiple Hyperscale databases in Azure SQL Database by using Hyperscale elastic pools. For one price, you can distribute resources where they're needed.
author: arvindshmicrosoft
ms.author: arvindsh
ms.reviewer: wiassaf, mathoma, randolphwest
ms.date: 01/03/2024
ms.service: sql-database
ms.subservice: elastic-pools
ms.custom: ignite-2023
ms.topic: conceptual
---
# Hyperscale elastic pools overview in Azure SQL Database

[!INCLUDE [appliesto-sqldb](../includes/appliesto-sqldb.md)]

This article provides an overview of Hyperscale elastic pools in Azure SQL Database.

An Azure SQL Database [elastic pool](elastic-pool-overview.md) enables software-as-a-service (SaaS) developers to optimize the price-performance ratio for a group of databases within a prescribed budget while delivering performance elasticity for each database. Azure SQL Database Hyperscale elastic pools introduces a shared resource model for Hyperscale databases.

For examples to create, scale, or move databases into a Hyperscale elastic pool by using the Azure CLI or PowerShell, review [Working with Hyperscale elastic pools using command-line tools](hyperscale-elastic-pool-command-line.md)

> [!NOTE]  
> [Elastic pools for Hyperscale](hyperscale-elastic-pool-overview.md) are currently in preview.

## Overview

Deploy your Hyperscale database to an [elastic pool](elastic-pool-overview.md) to share resources between databases within the pool and optimize the cost of having multiple databases with different usage patterns.

Scenarios to use an elastic pool with your Hyperscale databases:

- When you need to scale the compute resources allocated to the elastic pool up or down in a predictable amount of time, independent of the amount of allocated storage.
- When you want to scale out the compute resources allocated to the elastic pool by adding one or more read-scale replicas.
- If you want to use high transaction log throughput for write-intensive workloads, even with lower compute resources.

Migrating non-Hyperscale databases to a Hyperscale elastic pool upgrades the databases to the Hyperscale service tier.

## Architecture

Traditionally, the [architecture of a standalone Hyperscale database](service-tier-hyperscale.md#distributed-functions-architecture) consists of three main independent components: Compute, Storage ("Page Servers"), and the log ("Log Service"). When you create an elastic pool for your Hyperscale databases, the databases within the pool share compute and log resources. Additionally, if you choose to configure high availability, then each high availability pool is created with an equivalent and independent set of compute and log resources.

The following describes the architecture of an elastic pool for Hyperscale databases:

- A Hyperscale elastic pool consists of a primary pool that hosts primary Hyperscale databases and, if configured, up to four additional high-availability pools.
- Primary Hyperscale databases hosted in the primary elastic pool share the SQL Server database engine (sqlservr.exe) compute process, vCores, memory, and SSD cache.
- Configuring high availability for the primary pool creates additional high availability pools that contain read-only database replicas for the databases in the primary pool. Each primary pool can have a maximum of four high-availability replica pools. Each high-availability pool shares compute, SSD cache, and memory resources for all the secondary read-only databases in the pool.
- Hyperscale databases in the primary elastic pool all share the same log service. Since databases in the high availability pools don't have a write workload, they don't utilize the log service.
- Each Hyperscale database has its own set of page servers, and these page servers are shared between the primary database in the primary pool, and all secondary replica databases in the high availability pool.
- Geo-replicated secondary Hyperscale databases can be placed inside another elastic pool.
- Specifying `ApplicationIntent=ReadOnly` in your database connection string routes you to a read-only replica database in one of the high availability pools.

The following diagram shows the architecture of an elastic pool for Hyperscale databases:

:::image type="content" source="media/hyperscale-elastic-pool-overview/elastic-pool-hyperscale-architecture.png" alt-text="Diagram showing the Hyperscale elastic pool architecture.":::

## Manage Hyperscale elastic pool databases

You can use the [same commands](elastic-pool-manage.md) to manage your pooled Hyperscale databases as pooled databases in the other service tiers. Just be sure to specify `Hyperscale` for the edition when creating your Hyperscale elastic pool.

The only difference is the ability to modify the number of high availability (H/A) replicas for an existing Hyperscale elastic pool. To do so:

- Use the `HighAvailabilityReplicaCount` parameter of the Azure PowerShell [Set-AzSqlElasticPool](/powershell/module/az.sql/set-azsqlelasticpool) command.
- Use the `--ha-replicas` parameter of the Azure CLI [az sql elastic-pool update](/cli/azure/sql/elastic-pool) command.

You can use the following client tools to manage your Hyperscale databases in an elastic pool:

- Azure PowerShell: [Az.Sql.3.11.0 or higher](https://www.powershellgallery.com/packages/Az.Sql/3.11.0). PowerShell AzureRM.Sql isn't supported.
- The Azure CLI: [Az version 2.40.0 or higher](/cli/azure/install-azure-cli).
- Transact-SQL (T-SQL) starting with: [SQL Server Management Studio (SSMS) v18.12.1](/sql/ssms/download-sql-server-management-studio-ssms) or [Azure Data Studio v1.39.1](/azure-data-studio/download-azure-data-studio).

## Migrate non-Hyperscale databases to Hyperscale elastic pools

When migrating a database to Hyperscale, you can add the database to an existing Hyperscale elastic pool. For these migrations, the Hyperscale elastic pool needs to exist on the same logical server as the source database.

When migrating databases to Hyperscale elastic pools, be aware of the [maximum number of databases per Hyperscale elastic pool](#resource-limits).

### Migrate non-Hyperscale databases to Hyperscale elastic pools using T-SQL

You can use T-SQL commands to migrate multiple General Purpose databases and add them to an existing Hyperscale elastic pool named `hsep1`:

```sql
ALTER DATABASE gpepdb1 MODIFY (SERVICE_OBJECTIVE = ELASTIC_POOL(NAME = [hsep1]))
ALTER DATABASE gpepdb2 MODIFY (SERVICE_OBJECTIVE = ELASTIC_POOL(NAME = [hsep1]))
ALTER DATABASE gpepdb3 MODIFY (SERVICE_OBJECTIVE = ELASTIC_POOL(NAME = [hsep1]))
ALTER DATABASE gpepdb4 MODIFY (SERVICE_OBJECTIVE = ELASTIC_POOL(NAME = [hsep1]))
```

In this example, you're implicitly requesting a migration from General Purpose to Hyperscale, by specifying that the target `SERVICE_OBJECTIVE` is a Hyperscale elastic pool. Each of the above commands starts migrating the respective General Purpose database to Hyperscale. These `ALTER DATABASE` commands return quickly and don't wait for the migration to complete. In the example shown, you would have four such migrations from General Purpose to Hyperscale running in parallel. 

You can query the [sys.dm_operation_status](/sql/relational-databases/system-dynamic-management-views/sys-dm-operation-status-azure-sql-database?view=azuresqldb-current&preserve-view=true) dynamic management view to monitor the status of these background migration operations.

### Migrate non-Hyperscale databases to Hyperscale elastic pools using PowerShell

You can use PowerShell commands to migrate multiple General Purpose databases and add them to an existing Hyperscale elastic pool named `hsep1`. For example, the following sample script performs these steps:

1. Use the [Get-AzSqlElasticPoolDatabase](/powershell/module/az.sql/get-azsqlelasticpooldatabase) cmdlet to list all the databases in the General Purpose elastic pool named `gpep1`.
1. The `Where-Object` cmdlet filters the list to only those database names starting with `gpepdb`.
1. For each database, [Set-AzSqlDatabase](/powershell/module/az.sql/set-azsqldatabase) cmdlet starts a migration. In this case, you're implicitly requesting a migration to the Hyperscale service tier by specifying the target Hyperscale elastic pool named `hsep1`.
   - The `-AsJob` parameter allows each of the `Set-AzSqlDatabase` requests to run in parallel. If you prefer to run the migrations one-by-one, you can remove the `-AsJob` parameter.

```powershell
$dbs = Get-AzSqlElasticPoolDatabase -ResourceGroupName "myResourceGroup" -ServerName "mylogicalserver" -ElasticPoolName "gpep1"
$dbs | Where-Object { $_.DatabaseName -like "gpepdb*" } | % { Set-AzSqlDatabase -ResourceGroupName "myResourceGroup" -ServerName "mylogicalserver" -DatabaseName ($_.DatabaseName) -ElasticPoolName "hsep1" -AsJob }
```

In addition to the [sys.dm_operation_status](/sql/relational-databases/system-dynamic-management-views/sys-dm-operation-status-azure-sql-database?view=azuresqldb-current&preserve-view=true) dynamic management view, you can use the PowerShell cmdlet [Get-AzSqlDatabaseActivity](/powershell/module/az.sql/get-azsqldatabaseactivity) to monitor the status of these background migration operations.

## Resource limits

The following lists the supported limits for working with Hyperscale databases within elastic pools:

- Supported hardware generation: Standard-series (Gen5), premium-series, and premium-series memory optimized.
- vCore maximum per pool: 80 or 128 vCores, depending on the service level objective.
- Maximum supported data size per database: 100 TB.
- Maximum supported total data size across DBs in the pool: 100 TB.
- Maximum supported transaction log throughput per database: 100 MB.
- Maximum supported total transaction log throughput across databases in the pool: 131.25 MB/second.
- Each Hyperscale elastic pool can have up to 25 databases.

For greater detail, see the resource limits of Hyperscale elastic pools for [standard-series](resource-limits-vcore-elastic-pools.md#hyperscale---provisioned-compute---standard-series-gen5),  [premium-series](resource-limits-vcore-elastic-pools.md#hyperscale---premium-series), and [premium-series memory optimized](resource-limits-vcore-elastic-pools.md#hyperscale---premium-series-memory-optimized).

> [!NOTE]  
> Performance profiles, supported capabilities, and published limits are subject to change while the feature is in preview. As such, it's best to validate your use case with regular functional, performance, and scale testing of workloads.

## Limitations

Consider the following limitations:

- Changing an existing non-Hyperscale elastic pool to the Hyperscale edition isn't supported. The [migration section](#migrate-non-hyperscale-databases-to-hyperscale-elastic-pools) provides some alternatives you can use.
- Changing the edition of a Hyperscale elastic pool to a non-Hyperscale edition isn't supported.
- In order to [reverse migrate](./manage-hyperscale-database.md#reverse-migrate-from-hyperscale) an eligible database, which is in a Hyperscale elastic pool, it must first be removed from the Hyperscale elastic pool. The standalone Hyperscale database can then be reverse migrated to a General Purpose standalone database.
- Maintenance of databases in a pool is performed, and maintenance windows are configured, at the pool level. It isn't currently possible to configure a maintenance window for Hyperscale elastic pools.
- Zone redundancy isn't currently available for Hyperscale elastic pools. Attempting to add a zone-redundant Hyperscale database to a Hyperscale elastic pool results in an error.
- Adding a [named replica](./service-tier-hyperscale-replicas.md#named-replica) into a Hyperscale elastic pool isn't supported. Attempting to add a named replica of a Hyperscale database to a Hyperscale elastic pool results in an `UnsupportedReplicationOperation` error. Instead, create the named replica as a single Hyperscale database.

## Known issues

| Issue | Recommendation |
| *-- | *-- |
| If you try to create a new Hyperscale elastic pool from PowerShell with the `-ZoneRedundant` parameter specified, you get a vague `One or more errors occurred`. If you run the PowerShell command with the respective `-Verbose` and `-Debug` parameters specified, you get the actual error: `Provisioning of zone redundant database/pool is not supported for your current request`. | At this time, creating Hyperscale elastic pools with zone redundancy specified is unsupported. |
| In rare cases, you might get the error `45122 - This Hyperscale database cannot be added into an elastic pool at this time. In case of any questions, please contact Microsoft support`, when trying to move / restore / copy a Hyperscale database into an elastic pool. | This limitation is due to implementation-specific details. If this error is blocking you, raise a support incident and request help. |

## Related content

- [Working with Hyperscale elastic pools using command-line tools](hyperscale-elastic-pool-command-line.md)
- [Elastic pool pricing](https://azure.microsoft.com/pricing/details/sql-database/elastic)
- [Scale elastic pool resources in Azure SQL Database](elastic-pool-scale.md)
- [Use PowerShell to monitor and scale an elastic pool in Azure SQL Database](scripts/monitor-and-scale-pool-powershell.md)
- [Multi-tenant SaaS database tenancy patterns](saas-tenancy-app-design-patterns.md)
- [Introduction to a multitenant SaaS app that uses the database-per-tenant pattern with Azure SQL Database](saas-dbpertenant-wingtip-app-overview.md)
- [Resource management in dense elastic pools](elastic-pool-resource-management.md)
