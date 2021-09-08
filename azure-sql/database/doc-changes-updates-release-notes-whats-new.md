---
title: What's new? 
titleSuffix: Azure SQL Database
description: Learn about the new features and documentation improvements for Azure SQL Database.
services: sql-database
author: MashaMSFT
ms.author: mathoma
ms.service: sql-db-mi
ms.subservice: service-overview
ms.custom: sqldbrb=2, references_regions
ms.devlang: 
ms.topic: conceptual
ms.date: 09/01/2021
---
# What's new in Azure SQL Database
[!INCLUDE[appliesto-sqldb](../includes/appliesto-sqldb.md)]

This article summarizes the documentation changes associated with new features and improvements in the recent releases of [Azure SQL Database](https://azure.microsoft.com/products/azure-sql/database/). To learn more about Azure SQL Database, see the [overview](sql-database-paas-overview.md). 

For Azure SQL Managed Instance, see [What's new](../managed-instance/doc-changes-updates-release-notes-whats-new.md).


## Feature availability

The following two sections list the features of Azure SQL Database that are currently in public preview or have recently transitioned in general availability.

### Public preview 

The following table lists the features of Azure SQL Database that are currently in public preview. 

| Feature | Details |
| ---| --- |
| [Azure AD-only authentication](authentication-azure-ad-only-authentication.md) | It's possible to configure your Azure SQL Database to allow authentication only from Azure Active Directory. | 
| [Change data capture](/sql/relational-databases/track-changes/about-change-data-capture-sql-server) | Change data capture (CDC) lets you track all the changes that occur on a database. Though this feature has been available for SQL Server for quite some time, using it with Azure SQL Database is currently in public preview. |
| [Elastic jobs](elastic-jobs-overview.md) | The elastic jobs feature is the SQL Server Agent replacement for Azure SQL Database as a PaaS offering.  |
| [Elastic queries](elastic-query-overview.md) | The elastic queries feature allows for cross-database queries in Azure SQL Database. |
| [Elastic transactions](elastic-transactions-overview.md) | Elastic transactions can be distributed among cloud databases in Azure SQL Database and Azure SQL Managed Instance. |
| [Ledger](ledger-overview.md) | The Azure SQL Database ledger feature allows you to cryptographically attest to other parties, such as auditors or other business parties, that your data hasn't been tampered with. | 
| [Query editor in the Azure portal](connect-query-portal.md) | The query editor in the portal allows you to run queries against your Azure SQL Database directly from the [Azure portal](https://portal.azure.com).|
| [Query Store hints](/sql/relational-databases/performance/query-store-hints?view=azuresqldb-current&preserve-view=true) | Use query hints to optimize your query execution via the OPTION clause. |
| [SQL Analytics](../../azure-monitor/insights/azure-sql.md)|Azure SQL Analytics is an advanced cloud monitoring solution for monitoring performance of all of your Azure SQL databases at scale and across multiple subscriptions in a single view. Azure SQL Analytics collects and visualizes key performance metrics with built-in intelligence for performance troubleshooting.|
| [SQL insights](../../azure-monitor/insights/sql-insights-overview.md) |  SQL insights is a comprehensive solution for monitoring any product in the Azure SQL family. SQL insights uses dynamic management views to expose the data you need to monitor health, diagnose problems, and tune performance.| 
|||

### General availability (GA)

The following table lists the features of Azure SQL Database that have recently transitioned to general availability (GA). 

| Feature | GA Month | Details |
| ---| --- |--- |
| [AAD service principal](authentication-aad-service-principal.md) |  September 2021 | Azure Active Directory (Azure AD) supports user creation in Azure SQL Database on behalf of Azure AD applications (service principals)| 
| [AAD directory readers and guest users](authentication-aad-guest-users.md)  | September 2021  | Guest users in Azure Active Directory (Azure AD) are users that have been imported into the current Azure AD from other Azure Active Directories, or outside of it. | 
| [Audit management operations](../database/auditing-overview.md#auditing-of-microsoft-support-operations) |  March 2021 | Azure SQL audit capabilities enable you  you to audit operations done by Microsoft support engineers when they need to access your SQL assets during a support request, enabling more transparency in your workforce. | 
|||| 


## July 2021

| Changes | Details |
| --- | --- |
| **Azure AD-only authentication** | It's now possible to restrict authentication to your Azure SQL Database to Azure Active Directory users only. This feature is currently in public preview. To learn more, see [Azure AD-only authentication](authentication-azure-ad-only-authentication.md). | 
|||

## June 2021

| Changes | Details |
| --- | --- |
| **Query store hints** | It's now possible to use query hints to optimize your query execution via the OPTION clause. This feature is currently in public preview. To learn more, see [Query store hints](/sql/relational-databases/performance/query-store-hints?view=azuresqldb-current&preserve-view=true). | 
|||

## May 2021

| Changes | Details |
| --- | --- |
| **Change data capture** | Using change data capture (CDC) with Azure SQL Database is now in public preview. To learn more, see [Change data capture](/sql/relational-databases/track-changes/about-change-data-capture-sql-server). | 
| **SQL Database ledger** | SQL Database ledger is in public preview, and introduces the ability to cryptographically attest to other parties, such as auditors or other business parties, that your data hasn't been tampered with. To learn more, see [Ledger](ledger-overview.md). | 
|||

## Contribute to content

To contribute to the Azure SQL documentation, see the [Docs contributor guide](/contribute/).
