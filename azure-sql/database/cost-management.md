---
title: Plan and manage costs for Azure SQL Database
description: Learn how to plan for and manage costs for Azure SQL Database by using cost analysis in the Azure portal.
author: stevestein
ms.author: sstein
ms.custom: subject-cost-optimization
ms.service: sql-database
ms.topic: how-to
ms.date: 12/17/2020
---


# Plan and manage costs for Azure SQL Database

This article describes how you plan for and manage costs for Azure SQL Database. First, you use the Azure pricing calculator to help plan for Azure SQL Database costs before you add any resources for the service to estimate costs. Next, as you add Azure resources, review the estimated costs (if applicable to Azure SQL Database). After you've started using Azure SQL Database resources, use Cost Management features to set budgets and monitor costs. You can also review forecasted costs and identify spending trends to identify areas where you might want to act. Costs for Azure SQL Database are only a portion of the monthly costs in your Azure bill. Although this article explains how to plan for and manage costs for Azure SQL Database, you're billed for all Azure services and resources used in your Azure subscription, including the third-party services.


## Prerequisites

Cost analysis supports most Azure account types, but not all of them. To view the full list of supported account types, see [Understand Cost Management data](https://docs.microsoft.com/azure/cost-management-billing/costs/understand-cost-mgt-data?WT.mc_id=costmanagementcontent_docsacmhorizontal_-inproduct-learn). To view cost data, you need at least read access for an Azure account. 

For information about assigning access to Azure Cost Management data, see [Assign access to data](https://docs.microsoft.com/azure/cost-management/assign-access-acm-data?WT.mc_id=costmanagementcontent_docsacmhorizontal_-inproduct-learn).


## SQL Database initial cost considerations

When working with Azure SQL Database, there are several cost-saving features to consider:

- [Azure Hybrid Benefit (AHB)](../azure-hybrid-benefit.md)
- [Reserved capacity](reserved-capacity-overview.md)


### vCore or DTU purchasing models 

Azure SQL Database supports two purchasing models: vCore and DTU. The way you get charged varies between the purchasing models so it's important to understand the model that works best for your workload when planning and considering costs. For information about vCore and DTU purchasing models, see [Choose between the vCore and DTU purchasing models](purchasing-models.md).


### Provisioned or serverless

In the vCore purchasing model, Azure SQL Database also supports two types of compute tiers: provisioned throughput and serverless. The way you get charged for each compute tier varies so it's important to understand what works best for your workload when planning and considering costs. For details, see [vCore model overview - compute tiers](service-tiers-vcore.md#compute-tiers).

### Elastic pools

For environments with multiple databases that have varying and unpredictable usage demands, elastic pools can provide cost savings compared to provisioning the same amount of single databases. For details, see [Elastic pools](elastic-pool-overview.md).

## Estimate Azure SQL Database costs

- Use the [Azure pricing calculator](https://azure.microsoft.com/pricing/calculator/) to estimate costs before you add Azure SQL Database.

<!-- Note for Azure service writer: At a minimum, insert a brief walkthrough of using the calculator for your service. You don't need more than a couple of paragraphs. Add screenshots where useful. Add a screenshot where the estimated cost is shown. -->

<!--Note to Azure service writer: Replace the following example image with one specific to your service. -->

:::image type="content" source="media/cost-management/pricing-calc.png" alt-text="Azure SQL Database pricing calculator example":::

You can also estimate storage costs and how different Retention Policy options affect cost:

:::image type="content" source="media/cost-management/backup-storage.png" alt-text="Azure SQL Database pricing calculator example for storage":::


## Understand the full billing model for Azure SQL Database

Azure SQL Database runs on Azure infrastructure that accrues costs along with Azure SQL Database when you deploy the new resource. It's important to understand that additional infrastructure might accrue cost. You need to manage that cost when you make changes to deployed resources. 

<!--Note to Azure service writer: Include each of the following subsections at a minimum -->

Azure SQL Database (with the exception of serverless) is billed on a predictable, hourly rate. If the SQL database is active for less than one hour, you are billed for each hour the database exists using the highest service tier selected, provisioned storage and IO that applied during that hour, regardless of usage or whether the database was active for less than an hour.

### Costs that typically accrue with Azure SQL Database

<!--Note to Azure service writer: Include any costs that aren't obvious, hidden, or otherwise might not be present in the pricing calculator or resource creation experience in the Azure portal. You might need to sync with your product team to identify hidden costs. If you're certain that costs accrue only for your service and no others, then omit this section. -->

When you create resources for Azure SQL Database, resources for other Azure services are also created. They include:

- <OtherAzureService1>
- <OtherAzureService2>
 


### Using Monetary Credit with Azure SQL Database

<!--Note to Azure service writer: Let the user know that most 1st party Azure service charges can be fulfilled by EA monetary commitment credit. However, charges from third party products and services including those from the Azure Marketplace cannot be paid for by EA monetary commitment credit. -->

You can pay for Azure SQL Database charges with your EA monetary commitment credit. However, you can't use EA monetary commitment credit to pay for charges for third party products and services including those from the Azure Marketplace.

## Review estimated costs in the Azure portal

<!-- Note for Azure service writer: If your service shows estimated costs when a user is creating resources in the Azure portal, at a minimum, insert this section as a brief walkthrough that steps through creating a Azure SQL Database resource where the estimated cost is shown to the user, updated for your service. Add a screenshot where the estimated costs or subscription credits are shown.

If your service doesn't show costs as they create a resource or if estimated costs aren't shown to users before they use your service, then omit this section.

For example, you might start with the following (modify for your service):
-->

As you start using Azure SQL Database, you can see the estimated costs in the portal. Use the following steps to review the cost estimate:

1. Sign into the Azure portal and navigate to your Azure SQL database's resource group. You can locate teh resource group by navigating to your database and select **Resource group** in the **Overview** section.
1. In the menu, select **Cost analysis**.
1. View **Accumulated costs** and set the chart at the bottom to **Service name**. This chart shows an estimate of your current SQL Database costs. For more and information about the different cost analysis settings, see [Start analyzing costs](../../cost-management-billing/costs/quick-acm-cost-analysis.md).

You can see the costs as shown in the following screenshot:


:::image type="content" source="media/cost-management/cost-analysis.png" alt-text="Example showing accumulated costs in the Azure portal":::



If your Azure subscription has a spending limit, Azure prevents you from spending over your credit amount. As you create and use Azure resources, your credits are used. When you reach your credit limit, the resources that you deployed are disabled for the rest of that billing period. You can't change your credit limit, but you can remove it. For more information about spending limits, see [Azure spending limit](https://docs.microsoft.com/azure/billing/billing-spending-limit).

## Monitor costs

As you start using Azure SQL Database, you can see the estimated costs in the portal. Use the following steps to review the cost estimate:

1. Sign into the Azure portal and navigate to your Azure SQL database's resource group. You can locate teh resource group by navigating to your database and select **Resource group** in the **Overview** section.
1. In the menu, select **Cost analysis**.
1. View **Accumulated costs** and set the chart at the bottom to **Service name**. This chart shows an estimate of your current SQL Database costs. To narrow costs for the entire page to Azure SQL Database, select **Add filter** and then, select **Azure SQL Database**.

:::image type="content" source="media/cost-management/cost-analysis.png" alt-text="Example showing accumulated costs in the Azure portal":::

In the preceding example, you see the current cost for the service. Costs by Azure regions (locations) and Azure SQL Database costs by resource group are also shown. From here, you can explore costs on your own. For more and information about the different cost analysis settings, see [Start analyzing costs](https://docs.microsoft.com/azure/cost-management/cost-mgt-alerts-monitor-usage-spending?WT.mc_id=costmanagementcontent_docsacmhorizontal_-inproduct-learn).

## Create budgets

<!-- Note to Azure service writer: Modify the following as needed for your service. -->

You can create [budgets](https://docs.microsoft.com/azure/cost-management/tutorial-acm-create-budgets?WT.mc_id=costmanagementcontent_docsacmhorizontal_-inproduct-learn) to manage costs and create [alerts](https://docs.microsoft.com/azure/cost-management/cost-mgt-alerts-monitor-usage-spending?WT.mc_id=costmanagementcontent_docsacmhorizontal_-inproduct-learn) that automatically notify stakeholders of spending anomalies and overspending risks. Alerts are based on spending compared to budget and cost thresholds. Budgets and alerts are created for Azure subscriptions and resource groups, so they're useful as part of an overall cost monitoring strategy. 

Budgets can be created with filters for specific resources or services in Azure if you want more granularity present in your monitoring. Filters help ensure that you don't accidentally create new resources that cost you additional money. For more about the filter options when you when create a budget, see [Group and filter options](https://docs.microsoft.com/azure/cost-management-billing/costs/group-filter?WT.mc_id=costmanagementcontent_docsacmhorizontal_-inproduct-learn).

## Export cost data

You can also [export your cost data](https://docs.microsoft.com/azure/cost-management-billing/costs/tutorial-export-acm-data?WT.mc_id=costmanagementcontent_docsacmhorizontal_-inproduct-learn) to a storage account. This is helpful when you need or others to do additional data analysis for costs. For example, a finance teams can analyze the data using Excel or Power BI. You can export your costs on a daily, weekly, or monthly schedule and set a custom date range. Exporting cost data is the recommended way to retrieve cost datasets.


## Other ways to manage and reduce costs for Azure SQL Database

Azure SQL Database also enables you to scale resources up or down to control costs based on your application needs. For details, see [Dynamically scale database resources](scale-resources.md).

## Next steps

- Learn [how to optimize your cloud investment with Azure Cost Management](https://docs.microsoft.com/azure/cost-management-billing/costs/cost-mgt-best-practices?WT.mc_id=costmanagementcontent_docsacmhorizontal_-inproduct-learn).
- Learn more about managing costs with [cost analysis](https://docs.microsoft.com/azure/cost-management-billing/costs/quick-acm-cost-analysis?WT.mc_id=costmanagementcontent_docsacmhorizontal_-inproduct-learn).
- Learn about how to [prevent unexpected costs](https://docs.microsoft.com/azure/cost-management-billing/manage/getting-started?WT.mc_id=costmanagementcontent_docsacmhorizontal_-inproduct-learn).
- Take the [Cost Management](https://docs.microsoft.com/learn/paths/control-spending-manage-bills?WT.mc_id=costmanagementcontent_docsacmhorizontal_-inproduct-learn) guided learning course.

<!-- Insert links to other articles that might help users save and manage costs for you service here.

Create a table of contents entry for the article in the How-to guides section where appropriate. -->