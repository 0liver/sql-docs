---
title: Azure Monitor Logs extension for Azure Data Studio
description: This article describes how you can connect and query Log Analytics Workspace IDs with Azure Data Studio.
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: conceptual
author: markingmyname
ms.author: maghan
ms.reviewer: jukoesma
ms.custom: 
ms.date: 08/18/2021
---

# Azure Monitor Logs extension for Azure Data Studio (Preview)

The Azure Monitor Logs extension for [Azure Data Studio](../what-is-azure-data-studio.md) enables you to connect and query a [Log Analytics workspace](/azure/azure-monitor/essentials/quick-collect-activity-log-portal).

This extension is currently in preview.

## Prerequisites

If you don't have an Azure subscription, create a [free Azure account](https://azure.microsoft.com/free/) before you begin.

The following prerequisites are also required:

- [Azure Data Studio installed](../download-azure-data-studio.md).
- [Azure Monitor Logs Workspace](/azure/azure-sql/database/metrics-diagnostic-telemetry-logging-streaming-export-configure?tabs=azure-portal#stream-into-sql-analytics).

## Install the Azure Monitor Logs extension

To install the Azure Monitor Logs extension in Azure Data Studio, follow the steps below.

1. Open the extensions manager in Azure Data Studio. You can either select the extensions icon or select **Extensions** in the View menu.

2. Type in *Monitor logs* in the search bar.

3. Select the **Azure Monitor Logs** extension and view its details.

4. Select **Install**.

:::image type="content" source="media/azure-monitor-logs-extension/azure-monitor-logs-extension-icon.png" alt-text="Kusto extension":::

## How to connect to a Azure Monitor Log workspace

### Find your Azure Monitor Log workspace

Find your **Log Analytics Workspace** in the [Azure portal](https://ms.portal.azure.com/#home), then find the Workspace ID.

:::image type="content" source="media/azure-monitor-logs-extension/azure-monitor-logs-extension-workspace-id.png" alt-text="Azure Monitor Logs Workspace ID":::

### Connection details

To set up an Azure Monitor Log workspace, follow the steps below.

1. Select **New connection** from the **Connections** pane.

2. Fill in the **Connection Details** information.
    1. For **Connection type**, select *Azure Monitor Logs*.
    2. For **Workspace ID**, enter in your Log Analytics Workspace ID.
    3. For **Authentication Type**, use the default - *Azure Active Directory - Universal with MFA account*.
    4. For **Account**, use your account information.
    5. For **Database**, select the same Workspace ID.
    6. For **Server Group**, use *Default*.
        1. You can use this field to organize your servers in a specific group.
    7. For **Name (optional)**, leave blank.
        1. You can use this field to give your server an alias.

    :::image type="content" source="media/azure-monitor-logs-extension/azure-monitor-logs-extension-connection-details.png" alt-text="Connection details":::

For more information about writing Azure Monitor Logs, visit [Azure Monitor documentation](/azure/azure-monitor/)

## Next steps

- [Create and run a Kusto notebook](../notebooks/notebooks-kusto-kernel.md)
- [Kqlmagic notebook in Azure Data Studio](../notebooks/notebooks-kqlmagic.md)
- [SQL to Kusto cheat sheet](/azure/data-explorer/kusto/query/sqlcheatsheet)
- [What is Azure Data Explorer?](/azure/data-explorer/data-explorer-overview)
- [Using SandDance visualizations](https://sanddance.js.org/)
