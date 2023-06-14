---
title: Configure geo replication and backup restore for transparent data encryption (TDE) with database level customer-managed keys
titleSuffix: Azure SQL Database
description: A how-to guide for configuring geo replication for transparent data encryption (TDE) with database level customer-managed keys for Azure SQL Database.
author: strehan1993
ms.author: strehan
ms.reviewer: vanto
ms.date: 03/31/2023
ms.service: sql-database
ms.subservice: security
ms.custom: devx-track-azurecli, devx-track-azurepowershell
ms.topic: how-to
monikerRange: "= azuresql || = azuresql-db"
---

# Configure geo replication and backup restore for transparent data encryption with database level customer-managed keys

[!INCLUDE[appliesto-sqldb](../includes/appliesto-sqldb.md)]

> [!NOTE]
> Database Level TDE CMK is in public preview.
>
> This preview feature is available for Azure SQL Database (all SQL Database editions). It is not available for Azure SQL Managed Instance, SQL Server on-premises, Azure VMs, and Azure Synapse Analytics (dedicated SQL pools (formerly SQL DW)).

In this guide, we go through the steps to configure geo replication and backup restore on an Azure SQL Database. The Azure SQL Database is configured with transparent data encryption (TDE) and customer-managed keys (CMK) at the database level, utilizing a [user-assigned managed identity](/azure/active-directory/managed-identities-azure-resources/overview#managed-identity-types) to access [Azure Key Vault](/azure/key-vault/general/quick-create-portal). The Azure Key Vault is in an Azure Active Directory (Azure AD) that is in the same tenant as the Azure SQL logical server tenant.

## Prerequisites

- Have an Azure SQL Database configured with customer-managed keys at the database level, which is the source database for these operations. For more information, see [Transparent data encryption (TDE) with customer-managed keys at the database level](transparent-data-encryption-byok-database-level-overview.md).

> [!NOTE]
> The same guide can be applied to configure database level customer-managed keys in a different tenant by including the federated client ID parameter. For more information, see [Identity and key management for TDE with database level customer-managed keys](transparent-data-encryption-byok-database-level-basic-actions.md).

## Create an Azure SQL Database with database level customer-managed keys as a secondary or copy

Use the following commands to create a secondary replica or copy target of an Azure SQL Database configured with database level customer-managed keys. A user-assigned managed identity is required for setting up a customer-managed key for transparent data encryption during the database creation phase.

# [Azure CLI](#tab/azure-cli)

For information on installing the current release of Azure CLI, see [Install the Azure CLI](/cli/azure/install-azure-cli) article.

- Prepopulate the list of current keys in use by the primary database using the 'expand-keys' parameter with current as the 'keys-filter'

```azurecli
az sql db show --name $databaseName --resource-group $resourceGroup --server $serverName --expand-keys --keys-filter current
```

- Select the user-assigned managed identity (and federated client ID if configuring cross tenant access).
- Create a new database as a secondary and provide the prepopulated list of keys obtained from the source database and the above identity (and federated client ID if configuring cross tenant access).

```azurecli
# Create a secondary replica with Active Geo Replication with the same name as the primary database

az sql db replica create -g $resourceGroup -s $serverName -n $databaseName --partner-server $secondaryServer --partner-database $secondaryDatabase --partner-resource-group $secondaryResourceGroup -i --encryption-protector $encryptionProtector --user-assigned-identity-id $umi --keys $keys
```

> [!IMPORTANT]
> $keys is a space separated list of keys retrieved from the source database.

- To create a copy of the database, az sql db copy can be used with the same parameters.

```azurecli
# Create a copy of a database configured with database level customer-managed keys
az sql db copy -g $resourceGroup -s $serverName -n $databaseName --dest-name $secondaryDatabase -i --encryption-protector $encryptionProtector --user-assigned-identity-id $umi --keys $keys
```

# [PowerShell](#tab/azure-powershell)

For Az PowerShell module installation instructions, see [Install Azure PowerShell](/powershell/azure/install-az-ps). For specific cmdlets, see [AzureRM.Sql](/powershell/module/AzureRM.Sql/).

- Prepopulate the list of current keys in use by the primary database using the command [Get-AzSqlDatabase](/powershell/module/az.sql/get-azsqldatabase) and the `-ExpandKeyList` and `-KeysFilter "current"` parameters. Exclude `-KeysFilter` if you wish to retrieve all the keys.

```powershell
$database = Get-AzSqlDatabase -ResourceGroupName <ResourceGroupName> -ServerName <ServerName> -DatabaseName <DatabaseName> -ExpandKeyList -KeysFilter "current"
```

- Select the user-assigned managed identity (and federated client ID if configuring cross tenant access).
- Create a new database as a secondary using the command [New-AzSqlDatabaseSecondary](/powershell/module/az.sql/new-azsqldatabasesecondary) and provide the prepopulated list of keys obtained from the source database and the above identity (and federated client ID if configuring cross tenant access) in the API call using the `-KeyList`, `-AssignIdentity`, `-UserAssignedIdentityId`, `-EncryptionProtector ` (and if necessary, `-FederatedClientId`) parameters.

```powershell
# Create a secondary replica with Active Geo Replication with the same name as the primary database
$database = Get-AzSqlDatabase -ResourceGroupName <ResourceGroupName> -ServerName <ServerName> -DatabaseName <DatabaseName> -ExpandKeyList -KeysFilter "current"

$database | New-AzSqlDatabaseSecondary -PartnerResourceGroupName <SecondaryResourceGroupName> -PartnerServerName <SecondaryServerName> -AllowConnections "All" -AssignIdentity -UserAssignedIdentityId <UserAssignedIdentityId> -EncryptionProtector <CustomerManagedKeyId> -FederatedClientId <FederatedClientId>
-KeyList $database.Keys.Keys
```

- To create a copy of the database, [New-AzSqlDatabaseCopy](/powershell/module/az.sql/new-azsqldatabasecopy) can be used with the same parameters.

```powershell
# Create a copy of a database configured with database level customer-managed keys
$database = Get-AzSqlDatabase -ResourceGroupName <ResourceGroupName> -ServerName <ServerName> -DatabaseName <DatabaseName> -ExpandKeyList -KeysFilter "current"

New-AzSqlDatabaseCopy -CopyDatabaseName <CopyDatabaseName> -CopyResourceGroupName <CopyResourceGroupName> -CopyServerName <CopyServerName> -DatabaseName <DatabaseName> -ResourceGroupName <ResourceGroupName> -ServerName <ServerName> -AssignIdentity -UserAssignedIdentityId <UserAssignedIdentityId> -EncryptionProtector <CustomerManagedKeyId> -FederatedClientId <FederatedClientId>
-KeyList $database.Keys.Keys
```

# [ARM Template](#tab/arm-template)

Here's an example of an ARM template that creates a secondary replica and copy of an Azure SQL Database configured with a user-assigned managed identity and customer-managed TDE at the database level.

For more information and ARM templates, see [Azure Resource Manager templates for Azure SQL Database & SQL Managed Instance](arm-templates-content-guide.md).

Use a [Custom deployment in the Azure portal](https://portal.azure.com/#create/Microsoft.Template), and **Build your own template in the editor**. Next, **Save** the configuration once you pasted in the example.

- Prepopulate the list of current keys in use by the primary database using the following REST API request:

```rest
GET https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Sql/servers/{serverName}/databases/{databaseName}?api-version=2022-08-01-preview&$expand=keys($filter=pointInTime(‘current’))
```

- Select the user-assigned managed identity (and federated client ID if configuring cross tenant access).
- Create a new database as a secondary and provide the prepopulated list of keys obtained from the source database and the above identity (and federated client ID if configuring cross tenant access) in the ARM template as the `keys_to_add` parameter.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "server_name": {
      "type": "String"
    },
    "database_name": {
      "type": "String"
    },
    "user_assigned_identity": {
      "type": "String"
    },
    "encryption_protector": {
      "type": "String"
    },
    "location": {
      "type": "String"
    },
    "source_database_id": {
      "type": "String"
    },
    "keys_to_add": {
      "type": "Object"
    }
  },
  "variables": {},
  "resources": [
    {
      "type": "Microsoft.Sql/servers/databases",
      "apiVersion": "2022-08-01-preview",
      "name": "[concat(parameters('server_name'), concat('/',parameters('database_name')))]",
      "location": "[parameters('location')]",
      "sku": {
        "name": "Basic",
        "tier": "Basic",
        "capacity": 5
      },
      "identity": {
        "type": "UserAssigned",
        "userAssignedIdentities": {
          "[parameters('user_assigned_identity')]": {}
        }
      },
      "properties": {
        "collation": "SQL_Latin1_General_CP1_CI_AS",
        "maxSizeBytes": 104857600,
        "catalogCollation": "SQL_Latin1_General_CP1_CI_AS",
        "zoneRedundant": false,
        "readScale": "Disabled",
        "requestedBackupStorageRedundancy": "Geo",
        "maintenanceConfigurationId": "/subscriptions/e1775f9f-a286-474d-b6f0-29c42ac74554/providers/Microsoft.Maintenance/publicMaintenanceConfigurations/SQL_Default",
        "isLedgerOn": false,
        "encryptionProtector": "[parameters('encryption_protector')]",
        "keys": "[parameters('keys_to_add')]",
        "createMode": "Secondary",
        "sourceDatabaseId": "[parameters('source_database_id')]"
      }
    }
  ]
}
```

An example of the `encryption_protector` and `keys_to_add` parameter is:

```json
    "keys_to_add": {
      "value": {
        "https://yourvault.vault.azure.net/keys/yourkey1/fd021f84a0d94d43b8ef33154bca0000": {},
        "https://yourvault.vault.azure.net/keys/yourkey2/fd021f84a0d94d43b8ef33154bca0000": {}
      }
    },
    "encryption_protector": {
      "value": "https://yourvault.vault.azure.net/keys/yourkey2/fd021f84a0d94d43b8ef33154bca0000"
    }
```

- To create a copy of the database, the following template can be used with the same parameters.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "server_name": {
      "type": "String"
    },
    "database_name": {
      "type": "String"
    },
    "user_assigned_identity": {
      "type": "String"
    },
    "encryption_protector": {
      "type": "String"
    },
    "location": {
      "type": "String"
    },
    "source_database_id": {
      "type": "String"
    },
    "keys_to_add": {
      "type": "Object"
    }
  },
  "variables": {},
  "resources": [
    {
      "type": "Microsoft.Sql/servers/databases",
      "apiVersion": "2022-08-01-preview",
      "name": "[concat(parameters('server_name'), concat('/',parameters('database_name')))]",
      "location": "[parameters('location')]",
      "sku": {
        "name": "Basic",
        "tier": "Basic",
        "capacity": 5
      },
      "identity": {
        "type": "UserAssigned",
        "userAssignedIdentities": {
          "[parameters('user_assigned_identity')]": {}
        }
      },
      "properties": {
        "collation": "SQL_Latin1_General_CP1_CI_AS",
        "maxSizeBytes": 104857600,
        "catalogCollation": "SQL_Latin1_General_CP1_CI_AS",
        "zoneRedundant": false,
        "readScale": "Disabled",
        "requestedBackupStorageRedundancy": "Geo",
        "maintenanceConfigurationId": "/subscriptions/e1775f9f-a286-474d-b6f0-29c42ac74554/providers/Microsoft.Maintenance/publicMaintenanceConfigurations/SQL_Default",
        "isLedgerOn": false,
        "encryptionProtector": "[parameters('encryption_protector')]",
        "keys": "[parameters('keys_to_add')]",
        "createMode": "Copy",
        "sourceDatabaseId": "[parameters('source_database_id')]"
      }
    }
  ]
}
```

---

## Restore an Azure SQL Database with database level customer-managed keys

This section walks you through the steps to restore an Azure SQL Database configured with database level customer-managed keys. A user-assigned managed identity is required for setting up a customer-managed key for transparent data encryption during the database creation phase.

### Point in time restore

The following section describes how to restore a database configured with customer-managed keys at the database level to a given point in time. To learn more about backup recovery for SQL Database, see [Recover a database in SQL Database](recovery-using-backups.md).

# [Azure CLI](#tab/azure-cli2)

For information on installing the current release of Azure CLI, see [Install the Azure CLI](/cli/azure/install-azure-cli) article.

- Prepopulate the list of keys used by the primary database using the `expand-keys` parameter with your restore point in time as the `keys-filter`.

```azurecli
az sql db show --name $databaseName --resource-group $resourceGroup --server $serverName --expand-keys --keys-filter $timestamp
```

- Select the user-assigned managed identity (and federated client ID if configuring cross tenant access).
- Create a new database as a restore target and provide the prepopulated list of keys obtained from the source database and the above identity (and federated client ID if configuring cross tenant access).

```azurecli
# Create a restored database
az sql db restore --dest-name $destName --name $databaseName --resource-group $resourceGroup --server $serverName --subscription $subscriptionId --time $timestamp -i --encryption-protector $encryptionProtector --user-assigned-identity-id $umi --keys $keys
```

> [!IMPORTANT]
> $keys is a space separated list of keys retrieved from the source database.

# [PowerShell](#tab/azure-powershell2)

For Az PowerShell module installation instructions, see [Install Azure PowerShell](/powershell/azure/install-az-ps). For specific cmdlets, see [AzureRM.Sql](/powershell/module/AzureRM.Sql/).

- Prepopulate the list of keys used by the primary database using the command [Get-AzSqlDatabase](/powershell/module/az.sql/get-azsqldatabase) and the `-ExpandKeyList` and `-KeysFilter "2023-01-01"` parameters (`2023-01-01` is an example of the point in time you wish to restore the database to). Exclude `-KeysFilter` if you wish to retrieve all the keys.

```powershell
$database = Get-AzSqlDatabase -ResourceGroupName <ResourceGroupName> -ServerName <ServerName> -DatabaseName <DatabaseName> -ExpandKeyList -KeysFilter <Timestamp>
```

- Select the user-assigned managed identity (and federated client ID if configuring cross tenant access).
- Use the command [Restore-AzSqlDatabase](/powershell/module/az.sql/restore-azsqldatabase) with the `-FromPointInTimeBackup` parameter and provide the prepopulated list of keys obtained from the above steps and the above identity (and federated client ID if configuring cross tenant access) in the API call using the `-KeyList`, `-AssignIdentity`, `-UserAssignedIdentityId`, `-EncryptionProtector` (and if necessary, `-FederatedClientId`) parameters.

```powershell
$database = Get-AzSqlDatabase -ResourceGroupName <ResourceGroupName> -ServerName <ServerName> -DatabaseName <DatabaseName> -ExpandKeyList -KeysFilter <Timestamp>

# Create a restored database
Restore-AzSqlDatabase -FromPointInTimeBackup -PointInTime <Timestamp> -ResourceId '/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Sql/servers/{serverName}/databases/{databaseName}' -ResourceGroupName <ResourceGroupName> -ServerName <ServerName> -TargetDatabaseName <TargetDatabaseName> -KeyList $database.Keys.Keys -EncryptionProtector <EncryptionProtector> -UserAssignedIdentityId <UserAssignedIdentityId> -AssignIdentity
```

---

### Dropped database restore

The following section describes how to restore a deleted database that was configured with customer-managed keys at the database level. To learn more about backup recovery for SQL Database, see [Recover a database in SQL Database](recovery-using-backups.md).

# [Azure CLI](#tab/azure-cli2)

For information on installing the current release of Azure CLI, see [Install the Azure CLI](/cli/azure/install-azure-cli) article.

- Prepopulate the list of keys used by the dropped database using the `expand-keys` parameter. It's recommended to pass all the keys that the source database was using. You can also attempt a restore with the keys provided at deletion time by using the `keys-filter` parameter.

```azurecli
az sql db show-deleted --name $databaseName --resource-group $resourceGroup --server $serverName --restorable-dropped-database-id "databaseName,133201549661600000" --expand-keys
```

> [!IMPORTANT]
> `restorable-dropped-database-id` can be retrieved by listing all restorable dropped databases in the server and is of the format `databaseName,deletedTimestamp`.

- Select the user-assigned managed identity (and federated client ID if configuring cross tenant access).
- Create a new database as a restore target and provide the prepopulated list of keys obtained from the deleted source database and the above identity (and federated client ID if configuring cross tenant access).

```azurecli
# Create a restored database
az sql db restore --dest-name $destName --name $databaseName --resource-group $resourceGroup --server $serverName --subscription $subscriptionId --time $timestamp -i --encryption-protector $encryptionProtector --user-assigned-identity-id $umi --keys $keys --deleted-time "2023-02-06T11:02:46.160000+00:00"
```

> [!IMPORTANT]
> `$keys` is a space separated list of keys retrieved from the source database.

# [PowerShell](#tab/azure-powershell2)

For Az PowerShell module installation instructions, see [Install Azure PowerShell](/powershell/azure/install-az-ps). For specific cmdlets, see [AzureRM.Sql](/powershell/module/AzureRM.Sql/).

- Prepopulate the list of keys used by the primary database using the command [Get-AzSqlDeletedDatabaseBackup](/powershell/module/az.sql/get-azsqldeleteddatabasebackup) and the `-ExpandKeyList` parameter. It's recommended to pass all the keys that the source database was using. You can also attempt a restore with the keys provided at deletion time by using the `-KeysFilter` parameter.

```powershell
$database = Get-AzSqlDeletedDatabaseBackup -ResourceGroupName <ResourceGroupName> -ServerName <ServerName> -DatabaseId "dbName,133201549661600000" -ExpandKeyList -DeletionDate "2/6/2023" -DatabaseName <databaseName>
```

> [!IMPORTANT]
> `DatabaseId` can be retrieved by listing all restorable dropped databases in the server and is of the format `databaseName,deletedTimestamp`.

- Select the user-assigned managed identity (and federated client ID if configuring cross tenant access).
- Use the command [Restore-AzSqlDatabase](/powershell/module/az.sql/restore-azsqldatabase) with the `-FromDeletedDatabaseBackup` parameter and provide the prepopulated list of keys obtained from the above steps and the above identity (and federated client ID if configuring cross tenant access) in the API call using the `-KeyList`, `-AssignIdentity`, `-UserAssignedIdentityId`, `-EncryptionProtector` (and if necessary, `-FederatedClientId`) parameters.

```powershell
$database = Get-AzSqlDeletedDatabaseBackup -ResourceGroupName <ResourceGroupName> -ServerName <ServerName> -DatabaseId "dbName,133201549661600000" -ExpandKeyList -DeletionDate <DeletionDate> -DatabaseName <databaseName>

# Create a restored database
Restore-AzSqlDatabase -FromDeletedDatabaseBackup -DeletionDate <Timestamp> -ResourceId '/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Sql/servers/{serverName}/restorableDroppedDatabases/{databaseName}' -ResourceGroupName <ResourceGroupName> -ServerName <ServerName> -TargetDatabaseName <TargetDatabaseName> -KeyList $database.Keys.Keys -EncryptionProtector <EncryptionProtector> -UserAssignedIdentityId <UserAssignedIdentityId> -AssignIdentity
```

---

### Geo restore

The following section describes how to restore a geo-replicated backup of database that is configured with customer-managed keys at the database level. To learn more about backup recovery for SQL Database, see [Recover a database in SQL Database](recovery-using-backups.md).

# [Azure CLI](#tab/azure-cli2)

For information on installing the current release of Azure CLI, see [Install the Azure CLI](/cli/azure/install-azure-cli) article.

- Prepopulate the list of keys used by the geo backup of the database configured with customer-managed keys at the database level using the `expand-keys` parameter.

```azurecli
az sql db geo-backup --database-name $databaseName --g $resourceGroup --server $serverName  --expand-keys
```

- Select the user-assigned managed identity (and federated client ID if configuring cross tenant access).
- Create a new database as a geo restore target and provide the prepopulated list of keys obtained from the deleted source database and the above identity (and federated client ID if configuring cross tenant access).

```azurecli
# Create a geo restored database
az sql db geo-backup restore --geo-backup-id "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Sql/servers/{serverName}/recoverableDatabases/{databaseName}" --dest-database $destName --resource-group $resourceGroup --dest-server $destServerName -i --encryption-protector $encryptionProtector --user-assigned-identity-id $umi --keys $keys
```

> [!IMPORTANT]
> `$keys` is a space separated list of keys retrieved from the source database.

# [PowerShell](#tab/azure-powershell2)

For Az PowerShell module installation instructions, see [Install Azure PowerShell](/powershell/azure/install-az-ps). For specific cmdlets, see [AzureRM.Sql](/powershell/module/AzureRM.Sql/).

- Prepopulate the list of keys used by the primary database using the command [Get-AzSqlDatabaseGeoBackup](/powershell/module/az.sql/get-azsqldatabasegeobackup) and the `-ExpandKeyList` to retrieve all the keys.

```powershell
$database = Get-AzSqlDatabaseGeoBackup -ResourceGroupName <ResourceGroupName> -ServerName <ServerName> -DatabaseName <databaseName> -ExpandKeyList
```

> [!IMPORTANT]
> `DatabaseId` can be retrieved by listing all restorable dropped databases in the server and is of the format `databaseName,deletedTimestamp`.

- Select the user-assigned managed identity (and federated client ID if configuring cross tenant access).
- Use the command [Restore-AzSqlDatabase](/powershell/module/az.sql/restore-azsqldatabase) with the `-FromGeoBackup` parameter and provide the prepopulated list of keys obtained from the above steps and the above identity (and federated client ID if configuring cross tenant access) in the API call using the `-KeyList`, `-AssignIdentity`, `-UserAssignedIdentityId`, `-EncryptionProtector` (and if necessary, `-FederatedClientId`) parameters.

```powershell
$database = Get-AzSqlDatabaseGeoBackup -ResourceGroupName <ResourceGroupName> -ServerName <ServerName> -DatabaseName <databaseName> -ExpandKeyList

# Create a restored database
Restore-AzSqlDatabase -FromGeoBackup -ResourceId "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Sql/servers/{serverName}/recoverableDatabases/{databaseName}" -ResourceGroupName <ResourceGroupName> -ServerName <ServerName> -TargetDatabaseName <TargetDatabaseName> -KeyList $database.Keys.Keys -EncryptionProtector <EncryptionProtector> -UserAssignedIdentityId <UserAssignedIdentityId> -AssignIdentity
```

---

> [!IMPORTANT]
> Long term retention (LTR) backups don't provide the list of keys used by the backup. To restore an LTR backup, all the keys used by the source database must be passed to the LTR restore target.

> [!NOTE]
> The ARM template highlighted in the [Create an Azure SQL Database with database level customer-managed keys as a secondary or copy](#create-an-azure-sql-database-with-database-level-customer-managed-keys-as-a-secondary-or-copy) section can be referenced to restore the database with an ARM template by changing the `createMode` parameter.

## Next steps

Check the following documentation on various database level CMK operations:

- [Transparent data encryption (TDE) with customer-managed keys at the database level](transparent-data-encryption-byok-database-level-overview.md)

- [Identity and key management for TDE with database level customer-managed keys](transparent-data-encryption-byok-database-level-basic-actions.md)
