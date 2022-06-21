---
title: Change automated backup settings 
titleSuffix: Azure SQL Database
description: Change point in time restore and backup redundancy options for automatic backups in Azure SQL Database using the Azure portal, the Azure CLI, Azure PowerShell, and the Rest API. 
services:
  - "sql-database"
ms.service: sql-db-mi
ms.subservice: backup-restore
ms.custom:
  - "references_regions"
  - "devx-track-azurepowershell"
  - "devx-track-azurecli"
ms.topic: conceptual
author: SudhirRaparla
ms.author: nvraparl
ms.reviewer: wiassaf, mathoma, danil
ms.date: 04/26/2022
monikerRange: "= azuresql || = azuresql-db || = azuresql-mi"
---
# Change automated backup settings for Azure SQL Database
[!INCLUDE[appliesto-sqldb](../includes/appliesto-sqldb.md)]


> [!div class="op_single_selector"]
> * [Azure SQL Database](automated-backups-change-settings.md)
> * [Azure SQL Managed Instance](../managed-instance/automated-backups-change-settings.md)


This article provides examples to modify [automated backup settings](automated-backups-overview.md) for Azure SQL Database, such as the short-term retention policy and the backup storage redundancy option used for backups. For Azure SQL Managed Instance, see [Change backup settings](../managed-instance/automated-backups-change-settings.md). 


## Change short-term retention policy

You can change the default PITR backup retention period and the differential backup frequency by using the Azure portal, PowerShell, or the REST API. The following examples illustrate how to change the PITR retention to 28 days and the differential backups to 24 hour interval.

> [!WARNING]
> If you reduce the current retention period, you lose the ability to restore to points in time older than the new retention period. Backups that are no longer needed to provide PITR within the new retention period are deleted. If you increase the current retention period, you do not immediately gain the ability to restore to older points in time within the new retention period. You gain that ability over time, as the system starts to retain backups for longer.

> [!NOTE]
> These APIs will affect only the PITR retention period. If you configured LTR for your database, it won't be affected. For information about how to change LTR retention periods, see [Long-term retention](long-term-retention-overview.md).

### [Azure portal](#tab/azure-portal)

To change the PITR backup retention period or the differential backup frequency for active databases by using the Azure portal, go to the server or managed instance with the databases whose retention period you want to change. Select **Backups** in the left pane, then select the **Retention policies** tab. Select the database(s) for which you want to change the PITR backup retention. Then select **Configure retention** from the action bar.

![Change PITR retention, server level](./media/automated-backups-overview/configure-backup-retention-sqldb.png)

### [Azure CLI](#tab/azure-cli)

Prepare your environment for the Azure CLI.

[!INCLUDE[azure-cli-prepare-your-environment-no-header](../includes/azure-cli-prepare-your-environment-no-header.md)]

Change the PITR backup retention and differential backup frequency for active Azure SQL Databases by using the following example.

```azurecli
# Set new PITR differential backup frequency on an active individual database
# Valid backup retention must be between 1 and 35 days
# Valid differential backup frequency must be ether 12 or 24
az sql db str-policy set \
    --resource-group myresourcegroup \
    --server myserver \
    --name mydb \
    --retention-days 28 \
    --diffbackup-hours 24
```

### [PowerShell](#tab/powershell)

To change the PITR backup retention and differential backup frequency for active Azure SQL Databases, use the following PowerShell example.

```powershell
# SET new PITR backup retention period on an active individual database
# Valid backup retention must be between 1 and 35 days
Set-AzSqlDatabaseBackupShortTermRetentionPolicy -ResourceGroupName resourceGroup -ServerName testserver -DatabaseName testDatabase -RetentionDays 28
```

```powershell
# SET new PITR differential backup frequency on an active individual database
# Valid differential backup frequency must be ether 12 or 24. 
Set-AzSqlDatabaseBackupShortTermRetentionPolicy -ResourceGroupName resourceGroup -ServerName testserver -DatabaseName testDatabase -RetentionDays 28 -DiffBackupIntervalInHours 24
```


### [Rest API](#tab/rest-api)

#### Sample Request

```http
PUT https://management.azure.com/subscriptions/00000000-1111-2222-3333-444444444444/resourceGroups/resourceGroup/providers/Microsoft.Sql/servers/testserver/databases/testDatabase/backupShortTermRetentionPolicies/default?api-version=2021-02-01-preview
```

#### Request Body

```json
{ 
    "properties":{
        "retentionDays":28,
        "diffBackupIntervalInHours":24
  }
}
```

#### Sample Response: 

```json
{ 
  "id": "/subscriptions/00000000-1111-2222-3333-444444444444/providers/Microsoft.Sql/resourceGroups/resourceGroup/servers/testserver/databases/testDatabase/backupShortTermRetentionPolicies/default",
  "name": "default",
  "type": "Microsoft.Sql/resourceGroups/servers/databases/backupShortTermRetentionPolicies",
  "properties": {
    "retentionDays": 28,
    "diffBackupIntervalInHours":24
  }
}
```


For more information, see [Backup Retention REST API](/rest/api/sql/backupshorttermretentionpolicies).

---


## Configure backup storage redundancy

Backup storage redundancy for databases in Azure SQL Database can be configured at the time of database creation or can be updated for an existing database; the changes made to an existing database apply to future backups only. The default value is geo-redundant storage. For differences in pricing between locally redundant, zone-redundant and geo-redundant backup storage visit [SQL Database pricing page](https://azure.microsoft.com/pricing/details/sql-database/single/). 

Storage redundancy for Hyperscale databases is unique, review [Hyperscale backups storage redundancy](hyperscale-automated-backups-overview.md#backup-storage-redundancy) to learn more. 


### [Azure portal](#tab/azure-portal)

In the Azure portal, you can configure the backup storage redundancy on the **Create SQL Database** pane. The option is available under the Backup Storage Redundancy section. 

![Open Create SQL Database pane](./media/automated-backups-overview/sql-database-backup-storage-redundancy.png)

### [Azure CLI](#tab/azure-cli)

To configure backup storage redundancy when creating a new database, you can specify the `--backup-storage-redundancy` parameter with the `az sql db create` command. Possible values are `Geo`, `Zone`, and `Local`. By default, all databases in Azure SQL Database use geo-redundant storage for backups. Geo-restore is disabled if a database is created or updated with local or zone redundant backup storage.

This example creates a database in the [General Purpose](service-tier-general-purpose.md) service tier with local backup redundancy:

```azurecli
az sql db create \
    --resource-group myresourcegroup \
    --server myserver \
    --name mydb \
    --tier GeneralPurpose \
    --backup-storage-redundancy Local
```

Carefully consider the configuration option for `--backup-storage-redundancy` when creating a Hyperscale database. Storage redundancy can only be specified during the database creation process for Hyperscale databases. The selected storage redundancy option will be used for the lifetime of the database for both data storage redundancy and backup storage redundancy.  Learn more in [Hyperscale backups and storage redundancy](hyperscale-automated-backups-overview.md#backup-storage-redundancy). 

Existing Hyperscale databases can migrate to different storage redundancy using [database copy](database-copy.md) or point in time restore: sample code to copy a Hyperscale database follows in this section.

This example creates a database in the [Hyperscale](service-tier-general-purpose.md) service tier with Zone redundancy:

```azurecli
az sql db create \
    --resource-group myresourcegroup \
    --server myserver \
    --name mydb \
    --tier Hyperscale \
    --backup-storage-redundancy Zone
```

For more information, see [az sql db create](/cli/azure/sql/db#az-sql-db-create) and [az sql db update](/cli/azure/sql/db#az-sql-db-update).

Except for Hyperscale and Basic tier databases, you can update the backup storage redundancy setting for an existing database with the `--backup-storage-redundancy` parameter and the `az sql db update` command. It may take up to 48 hours for the changes to be applied on the database. Switching from geo-redundant backup storage to local or zone redundant storage disables geo-restore.

This example code changes the backup storage redundancy to `Local`.

```azurecli
az sql db update \
    --resource-group myresourcegroup \
    --server myserver \
    --name mydb \
    --backup-storage-redundancy Local
```

You cannot update the backup storage redundancy of a Hyperscale database directly. However, you can change it using [the database copy command](database-copy.md) with the `--backup-storage-redundancy` parameter. This example copies a Hyperscale database to a new database using Gen5 hardware and two vCores. The new database has the backup redundancy set to `Zone`.

```azurecli
az sql db copy \
    --resource-group myresourcegroup \ 
    --server myserver 
    --name myHSdb 
    --dest-resource-group mydestresourcegroup 
    --dest-server destdb 
    --dest-name myHSdb 
    --service-objective HS_Gen5_2 
    --read-replicas 0 
    --backup-storage-redundancy Zone
```

For syntax details, see [az sql db copy](/cli/azure/sql/db#az-sql-db-copy). For an overview of database copy, visit [Copy a transactionally consistent copy of a database in Azure SQL Database](database-copy.md).

### [PowerShell](#tab/powershell)

To configure backup storage redundancy when creating a new database, you can specify the `-BackupStorageRedundancy` parameter with the `New-AzSqlDatabase` cmdlet. Possible values are `Geo`, `Zone`, and `Local`. By default, all databases in Azure SQL Database use geo-redundant storage for backups. Geo-restore is disabled if a database is created with local or zone redundant backup storage.

This example creates a database in the [General Purpose](service-tier-general-purpose.md) service tier with local backup redundancy:

```powershell
# Create a new database with geo-redundant backup storage.  
New-AzSqlDatabase -ResourceGroupName "ResourceGroup01" -ServerName "Server01" -DatabaseName "Database03" -Edition "GeneralPurpose" -Vcore 2 -ComputeGeneration "Gen5" -BackupStorageRedundancy Local
```

Carefully consider the configuration option for `--backup-storage-redundancy` when creating a Hyperscale database. Storage redundancy can only be specified during the database creation process for Hyperscale databases. The selected storage redundancy option will be used for the lifetime of the database for both data storage redundancy and backup storage redundancy.  Learn more in [Hyperscale backups and storage redundancy](hyperscale-automated-backups-overview.md#backup-storage-redundancy).

Existing databases can migrate to different storage redundancy using [database copy](database-copy.md) or point in time restore: sample code to copy a Hyperscale database follows in this section.

This example creates a database in the [Hyperscale](service-tier-general-purpose.md) service tier with Zone redundancy:

```powershell
# Create a new database with geo-redundant backup storage.  
New-AzSqlDatabase -ResourceGroupName "ResourceGroup01" -ServerName "Server01" -DatabaseName "Database03" -Edition "Hyperscale" -Vcore 2 -ComputeGeneration "Gen5" -BackupStorageRedundancy Zone
```

For syntax details visit [New-AzSqlDatabase](/powershell/module/az.sql/new-azsqldatabase).

Except for Hyperscale and Basic tier databases, you can use the `-BackupStorageRedundancy` parameter with the `Set-AzSqlDatabase` cmdlet to update the backup storage redundancy setting for an existing database. Possible values are Geo, Zone, and Local. It may take up to 48 hours for the changes to be applied on the database. Switching from geo-redundant backup storage to local or zone redundant storage disables geo-restore.

This example code changes the backup storage redundancy to `Local`.

```powershell
# Change the backup storage redundancy for Database01 to zone-redundant. 
Set-AzSqlDatabase -ResourceGroupName "ResourceGroup01" -DatabaseName "Database01" -ServerName "Server01" -BackupStorageRedundancy Local
```

For details visit [Set-AzSqlDatabase](/powershell/module/az.sql/set-azsqldatabase)

Backup storage redundancy of an existing Hyperscale database cannot be updated. However, you can use the [database copy command](database-copy.md) to create a copy of the database and use the `-BackupStorageRedundancy` parameter to update the backup storage redundancy. This example copies a Hyperscale database to a new database using Gen5 hardware and two vCores. The new database has the backup redundancy set to `Zone`.

```powershell
# Change the backup storage redundancy for Database01 to zone-redundant. 
New-AzSqlDatabaseCopy -ResourceGroupName "ResourceGroup01" -ServerName "Server01" -DatabaseName "HSSourceDB" -CopyResourceGroupName "DestResourceGroup" -CopyServerName "DestServer" -CopyDatabaseName "HSDestDB" -Vcore 2 -ComputeGeneration "Gen5" -ComputeModel Provisioned -BackupStorageRedundancy Zone
```

For syntax details, visit [New-AzSqlDatabaseCopy](/powershell/module/az.sql/new-azsqldatabasecopy). 

For an overview of database copy, visit [Copy a transactionally consistent copy of a database in Azure SQL Database](database-copy.md).

> [!NOTE]
> To use -BackupStorageRedundancy parameter with database restore, database copy or create secondary operations, use Azure PowerShell version Az.Sql 2.11.0. 



### [Rest API](#tab/rest-api)

It's not currently possible to change the backup storage redundancy using the Rest API. 

---



## Next steps

- Database backups are an essential part of any business continuity and disaster recovery strategy because they protect your data from accidental corruption or deletion. To learn about the other SQL Database business continuity solutions, see [Business continuity overview](business-continuity-high-availability-disaster-recover-hadr-overview.md).
- For information about how to configure, manage, and restore from long-term retention of automated backups in Azure Blob storage by using the Azure portal, see [Manage long-term backup retention by using the Azure portal](long-term-backup-retention-configure.md).
- For information about how to configure, manage, and restore from long-term retention of automated backups in Azure Blob storage by using PowerShell, see [Manage long-term backup retention by using PowerShell](long-term-backup-retention-configure.md). 
- Get more information about how to [restore a database to a point in time by using the Azure portal](recovery-using-backups.md).
- Get more information about how to [restore a database to a point in time by using PowerShell](scripts/restore-database-powershell.md).
- To learn all about backup storage consumption on Azure SQL Managed Instance, see [Backup storage consumption on Managed Instance explained](https://aka.ms/mi-backup-explained).
- To learn how to fine-tune backup storage retention and costs for Azure SQL Managed Instance, see [Fine tuning backup storage costs on Managed Instance](https://aka.ms/mi-backup-tuning).
