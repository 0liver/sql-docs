---
title: Automatically connect your SQL Server to Azure Arc
description: Automatically connect an instance of SQL Server to Azure Arc. Allows you to manage SQL Server centrally, as an Arc-enabled resource.
author: anosov1960
ms.author: rajpo
ms.reviewer: mikeray, maghan
ms.date: 07/18/2023
ms.service: sql
ms.topic: conceptual
---

# Automatically connect your SQL Server to Azure Arc

[!INCLUDE [sqlserver](../../includes/applies-to-version/sqlserver.md)]

This article explains how to connect your SQL Server instance to Azure Arc. 

Before you proceed:

- Verify your server meets [prerequisites for the connected machine agent](/azure/azure-arc/servers/prerequisites).
- Verify your network and firewall meets the [network requirements for connected machine agent](/azure/azure-arc/servers/network-requirements).

## Steps

1. [Onboard the server to Azure Arc](#onboard-the-server-to-azure-arc)
1. Wait approximately 10 minutes
1. [Verify your Azure Arc-enabled SQL Server resources](#verify-your-azure-arc-enabled-sql-server-resources)

## Onboard the server to Azure Arc

To connect your SQL Server to Azure, see [Azure Connected Machine agent deployment options](/azure/azure-arc/servers/deployment-options).

> [!NOTE]  
> If your server is already connected to Azure, but Azure extension for SQL Server is not deployed automatically using above methods, proceed to [Alternate deployment options for Azure Arc-enabled SQL Server](deployment-options.md) to connect SQL Server to Azure.

Azure Arc automatically installs the Azure extension for SQL Server when a server connected to Azure Arc has SQL Server installed. All the SQL Server instance resources are automatically created in Azure. These provide a centralized management platform for all your SQL Servers.

To learn more, see [Auto deployment of Azure extension for SQL Server](connect-at-scale-autodeploy.md).

## Verify your Azure Arc-enabled SQL Server resources

In Azure portal, go to **Azure Arc > SQL Server** and open the newly registered Arc-enabled SQL Server resource to validate.

   :::image type="content" source="media/join/validate-sql-server-azure-arc.png" alt-text="Screenshot of validating a connected SQL Server.":::

> [!NOTE]  
> After connecting the server to Arc, you need to wait 10 minutes for the Azure extension for SQL Server to deploy and the Arc-enabled SQL Server resource to be created.
>

If the Arc-enabled SQL Server resource does not appear in the same resource group as the Arc Server, follow the [Azure extension for SQL Server](troubleshoot-deployment.md) guide to troubleshoot the issue.

## Evaluate Azure Arc-enabled SQL Server on an Azure VM

Normally, you wouldn't use Azure Arc-enabled SQL Server on an Azure virtual machine. However, you can use an Azure SQL Virtual machine to evaluate Arc-enabled SQL Server on an Azure VM.

To test Arc-enabled SQL Server on an Azure VM, follow the steps at [Evaluate Arc-enabled servers on Azure virutal machine](/azure/azure-arc/servers/plan-evaluate-on-azure-virtual-machine). For you Azure VM image, use an [available Azure SQL VM image](/azure/azure-sql/virtual-machines/windows/sql-vm-create-portal-quickstart).

## Related content

- [Configure advanced data security for your SQL Server instance](configure-advanced-data-security.md)
- [Configure best practices assessment on an Azure Arc-enabled SQL Server instance](assess.md)
