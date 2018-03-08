---
title: "SQL Server vNext Release Notes | Microsoft Docs"
ms.custom: ""
ms.date: "03/07/2018"
ms.prod: "sql-server-2018"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "server-general"
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: 13942af8-5a40-4cef-80f5-918386767a47
author: "craigg-msft"
ms.author: "craigg"
manager: "jhubbard"
---
# SQL Server vNext Release Notes
This article describes limitations and issues with SQL Server vNext on Windows and SQL Server vNext on Linux. For related information, see:
- [What's New in SQL Server vNext](../sql-server/what-s-new-in-sql-server-vnext.md)

**Try SQL Server!**
[![Download from Evaluation Center](../includes/media/download2.png)](http://go.microsoft.com/fwlink/?LinkID=829477) [Download SQL Server vNext](http://go.microsoft.com/fwlink/?LinkID=829477)

## SQL Server vNext Community Technology Preview (CTP 1.4 - March 2018)

### Master Data Services (MDS)
**Issue and customer impact:** In the Master Data Services (MDS) portal, the Silverlight components on the following pages have been replaced with HTML controls:
-   Function Explore
    -   Entities
    -   Entity Dependencies
    -   Hierarchies
-   Function System Administration 
    -   Manage Derived Hierarchies - Edit Derived Hierarchy - Preview 

On the modified **Explore** pages, the operators `Matches` and `Does not match` are not supported for filtering grid data in the function **Filter**. Support for these operators will be restored in later CTP releases.

**Workaround:** No workaround is available.

**Applies to:** Windows

[!INCLUDE[get-help-options-msft-only](../includes/paragraph-content/get-help-options.md)]

![MS_Logo_X-Small](../sql-server/media/ms-logo-x-small.png)