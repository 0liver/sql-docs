---
title: Create SQL Server VM (Azure Resource Manager template)
description: Learn how to create a SQL Server on Azure Virtual Machine (VM) by using Azure Resource Manager template.
services: azure-resource-manager
author: MashaMSFT
ms.service: azure-resource-manager
ms.topic: quickstart
ms.custom: subject-armqs
ms.author: mathoma
ms.date: 06/16/20
ms.service: virtual-machines-sql
---

# Create SQL Server VM (Azure Resource Manager template)

[!INCLUDE [About Azure Resource Manager](../../../../includes/resource-manager-quickstart-introduction.md)]

## Prerequisites

The SQL VM ARM template requires the following:

- A [resource group](../../../azure-resource-manager/management/manage-resource-groups-portal.md#create-resource-groups) with a [virtual network](../../../virtual-network/quick-create-portal.md) and [subnet](../../../virtual-network/virtual-network-manage-subnet#add-a-subnet) already preconfigured. 

If you don't have an Azure subscription, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) before you begin.


## Review the template

The template used in this quickstart is from [Azure Quickstart Templates](https://azure.microsoft.com/resources/templates/101-sql-vm-new-storage/).


:::code language="json" source="~/quickstart-templates/101-sql-vm-new-storage/azuredeploy.json" range="000-000" highlight="169-310":::

Five Azure resources are defined in the template: 

- [Microsoft.Network/publicIpAddresses](/azure/templates/microsoft.network/2018-08-01/publicipaddresses): Creates a public IP address. 
- [Microsoft.Network/networkSecurityGroups](/azure/templates/microsoft.network/networksecuritygroups): Creates a network security group. 
- [Microsoft.Network/networkInterfaces](/azure/templates/microsoft.network/networkinterfaces): Configures the network interface. 
- [Microsoft.Compute/virtualMachines](/azure/templates/microsoft.compute/virtualmachines): Creates a virtual machine in Azure. 
- [Microsoft.SqlVirtualMachine/SqlVirtualMachines](/azure/templates/microsoft.sqlvirtualmachine/sqlvirtualmachines): registers the virtual machine with the SQL VM resource provider. 

More SQL Server on Azure VM templates can be found in the [quickstart template gallery](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Sqlvirtualmachine).


## Deploy the template

1. Select the following image to sign in to Azure and open a template. The template creates an Azure Cosmos account, a database, and a container.

   [![Deploy to Azure](../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3a%2f%2fraw.githubusercontent.com%2fAzure%2fazure-quickstart-templates%2fmaster%2f101-sql-vm-new-storage%2fazuredeploy.json)

2. Select or enter the following values.

    * **Subscription**: select an Azure subscription.
    * **Resource group**: The prepared resource group for your SQL Server VM. 
    * **Region**: select a region.  For example, **Central US**.
    * **Virtual Machine Name**: enter a name for SQL Server virtual machine. 
    * **Virtual Machine Size**: Choose the appropriate size for your virtual machine from the drop-down.
    * **Existing Virtual Network Name**: The primary replica region for the Azure Cosmos account.
    * **Existing Vnet Resource Group**: The secondary replica region for the Azure Cosmos account.
    * **Existing Subnet Name**: The default consistency level for the Azure Cosmos account.
    * **Image Offer**: Max stale requests. Required for BoundedStaleness.
    * **SQL Sku**: Max lag time. Required for BoundedStaleness.
    * **Admin Username**: The name of the Azure Cosmos database.
    * **Admin Password**: The name of the Azure Cosmos container.
    * **Storage Workload Type**:  The throughput for the container, minimum throughput value is 400 RU/s.
    * **Sql Data Disks Count**:  The throughput for the container, minimum throughput value is 400 RU/s.
    * **Data Path**:  The throughput for the container, minimum throughput value is 400 RU/s.
    * **Sql Log Disks Count**:  The throughput for the container, minimum throughput value is 400 RU/s.
    * **Log Path**:  The throughput for the container, minimum throughput value is 400 RU/s.
    * **Location**:  The throughput for the container, minimum throughput value is 400 RU/s.

3. Select **Review + create **. After the Azure Cosmos account has been deployed successfully, you get a notification:

   ![Resource Manager template, Cosmos DB integration, deploy portal notification](./media/quick-create-template/resource-manager-template-portal-deployment-notification.png)

The Azure portal is used to deploy the template. In addition to the Azure portal, you can also use the Azure PowerShell, Azure CLI, and REST API. To learn other deployment methods, see [Deploy templates](../azure-resource-manager/templates/deploy-powershell.md).

## Review deployed resources

You can either use the Azure portal or the Azure CLI  to check the deployed resources. 


```azurecli-interactive
echo "Enter the resource group where your SQL Server VM exists:" &&
read resourcegroupName &&
az resource list --resource-group $resourcegroupName 
```


## Clean up resources

When no longer needed, delete the resource group by using Azure CLI or Azure PowerShell:

# [CLI](#tab/CLI)

```azurecli-interactive
echo "Enter the Resource Group name:" &&
read resourceGroupName &&
az group delete --name $resourceGroupName &&
echo "Press [ENTER] to continue ..."
```

# [PowerShell](#tab/PowerShell)

```azurepowershell-interactive
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
Remove-AzResourceGroup -Name $resourceGroupName
Write-Host "Press [ENTER] to continue..."
```

---

## Next steps

For a step-by-step tutorial that guides you through the process of creating a template, see:

> [!div class="nextstepaction"]
> [ Tutorial: Create and deploy your first Azure Resource Manager template](/azure/azure-resource-manager/templates/template-tutorial-create-first-template)

For other ways to deploy a SQL Server VM, see: 
- [Azure portal](create-sql-vm-portal.md)
- [PowerShell](create-sql-vm-powershell.md)

To learn more, see [an overview of SQL Server on Azure VMs](sql-server-on-azure-vm-iaas-what-is-overview.md)