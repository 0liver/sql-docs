---
title: "Create a Basic Table Report (SSRS Tutorial) | Microsoft Docs"
ms.date: 04/16/2019
ms.prod: reporting-services
ms.prod_service: "reporting-services-native"
ms.technology: reporting-services

ms.topic: conceptual
helpviewer_keywords: 
  - "walkthroughs [Reporting Services]"
  - "tutorials [Reporting Services]"
  - "reports [Reporting Services], creating"
ms.assetid: 3b539b4b-26f2-4c0b-b506-80f175679a46
author: markingmyname
ms.author: maghan
---
# Create a Basic Table Report (SSRS Tutorial)

In this tutorial, you use Report Designer in SQL Server Data Tools to create a basic [!INCLUDE[ssRSnoversion_md](../includes/ssrsnoversion-md.md)] paginated report with a table, based on the **[!INCLUDE[ssSampleDBAdventureworks2017_md](../includes/sssampledbadventureworks2017-md.md)]** database. You can also create [!INCLUDE[ssRSnoversion_md](../includes/ssrsnoversion-md.md)] paginated reports with Report Builder. 

As you go through this tutorial, you will create a report project, set up connection information, define a query, add a Table data region, group and total some fields, and preview the report.  
  
## Requirements  
Your system must have the following installed to use this tutorial:  
  
<!-- Tutorial is now MSSQL2017 & VS2019 specific [!INCLUDE[msCoName](../includes/msconame-md.md)] SQL Server database engine. -->
//]: # (Please verify @mblythe as Blame Annotations say the reference to SQL version was removed 2yrs. ago)
-   [!INCLUDE[ssSql17](../includes/sssql17-md.md)] database engine.  
-   [!INCLUDE[ssRSCurrent](../includes/ssrscurrent-md.md)].
-   The [!INCLUDE[ssSampleDBAdventureworks2017_md](../includes/sssampledbadventureworks2017-md.md)] database.  For more information, see [Adventure Works Sample Databases)](https://github.com/Microsoft/sql-server-samples/releases).  
  
 -   [SQL Server Data Tools](../ssdt/download-sql-server-data-tools-ssdt.md) with the "SQL Server Reporting Services" Visual Studio Extension installed, which now installs automatically by selecting the "Data Storage and Processing Workload" during installation, then installing the Report Services Extension, so you have the Report Designer.
  
You must also have read-only permissions to retrieve data from the [!INCLUDE[ssSampleDBAdventureworks2017](../includes/sssampledbadventureworks2017-md.md)] database.

**Estimated time to complete the tutorial:** 30 minutes.
  
## Tasks  
[Lesson 1: Creating a Report Server Project &#40;Reporting Services&#41;](../reporting-services/lesson-1-creating-a-report-server-project-reporting-services.md)  
  
[Lesson 2: Specifying Connection Information &#40;Reporting Services&#41;](../reporting-services/lesson-2-specifying-connection-information-reporting-services.md)  
  
[Lesson 3: Defining a Dataset for the Table Report &#40;Reporting Services&#41;](../reporting-services/lesson-3-defining-a-dataset-for-the-table-report-reporting-services.md)  
  
[Lesson 4: Adding a Table to the Report &#40;Reporting Services&#41;](../reporting-services/lesson-4-adding-a-table-to-the-report-reporting-services.md)  
  
[Lesson 5: Formatting a Report &#40;Reporting Services&#41;](../reporting-services/lesson-5-formatting-a-report-reporting-services.md)  
  
[Lesson 6: Adding Grouping and Totals &#40;Reporting Services&#41;](../reporting-services/lesson-6-adding-grouping-and-totals-reporting-services.md)  

## Next steps

[Reporting Services Tutorials](../reporting-services/reporting-services-tutorials-ssrs.md)  

More questions? [Try asking the Reporting Services forum](https://go.microsoft.com/fwlink/?LinkId=620231)
