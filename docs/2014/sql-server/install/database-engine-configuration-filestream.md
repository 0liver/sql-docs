---
title: "Database Engine Configuration - Filestream | Microsoft Docs"
ms.custom: ""
ms.date: "06/14/2017"
ms.prod: "sql-server-2014"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "database-engine"
ms.tgt_pltfrm: ""
ms.topic: "article"
helpviewer_keywords: 
  - "FILESTREAM [SQL Server], about FILESTREAM"
ms.assetid: 641a10a1-ae52-4d26-8f1c-a032a4aeff02
caps.latest.revision: 12
author: "JennieHubbard"
ms.author: "jhubbard"
manager: "jhubbard"
---
# Database Engine Configuration - Filestream
  Use this page to enable FILESTREAM for this installation of [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]. FILESTREAM integrates the [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] with an NTFS file system by storing `varbinary(max)` binary large object (BLOB) data as files on the file system. [!INCLUDE[tsql](../../includes/tsql-md.md)] statements can insert, update, query, search, and back up FILESTREAM data. Win32 file system interfaces provide streaming access to the data.  
  
## UIElement List  
 **Enable FILESTREAM for Transact-SQL access**  
 Select to enable FILESTREAM for [!INCLUDE[tsql](../../includes/tsql-md.md)] access. This control must be checked before the other control options will be available.  
  
 **Enable FILESTREAM for file I/O streaming access**  
 Select to enable Win32 streaming access for FILESTREAM.  
  
 **Windows share name**  
 Use this control to enter the name of the Windows share in which the FILESTREAM data will be stored.  
  
 **Allow remote clients to have streaming access to FILESTREAM data**  
 Select this control to allow remote clients to access this FILESTREAM data on this server.  
  
## See Also  
 [Enable and Configure FILESTREAM](../../../2014/database-engine/enable-and-configure-filestream.md)   
 [sp_configure &#40;Transact-SQL&#41;](~/relational-databases/system-stored-procedures/sp-configure-transact-sql.md)  
  
  