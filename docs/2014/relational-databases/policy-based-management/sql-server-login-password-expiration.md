---
title: "SQL Server Login Password Expiration | Microsoft Docs"
ms.custom: ""
ms.date: "06/13/2017"
ms.prod: "sql-server-2014"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dbe-cross-instance"
ms.tgt_pltfrm: ""
ms.topic: "article"
helpviewer_keywords: 
  - "Best Practices [Database Engine]"
ms.assetid: 7e3bf9da-a436-433d-847a-47c30428cad3
caps.latest.revision: 8
author: "JennieHubbard"
ms.author: "jhubbard"
manager: "jhubbard"
---
# SQL Server Login Password Expiration
  This rule checks whether "Password expiration" of each [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] login is enabled. If [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Authentication is enabled and if the operating system version is earlier than [!INCLUDE[winxpsvr](../../includes/winxpsvr-md.md)], an attacker could repeatedly exploit a known [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] login password.  
  
## Best Practices Recommendations  
 We recommend that you upgrade the operating system to [!INCLUDE[winxpsvr](../../includes/winxpsvr-md.md)].  
  
 If [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Authentication is not required in your environment, use Windows Authentication. For more information, see [Choose an Authentication Mode](../security/choose-an-authentication-mode.md).  
  
 Enable "Password expiration" for all the [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] logins. Use [ALTER LOGIN](~/t-sql/statements/alter-login-transact-sql.md) to configure the password policy for the [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] login.  
  
## For More Information  
 [Password Policy](../security/password-policy.md)  
  
## See Also  
 [Monitor and Enforce Best Practices by Using Policy-Based Management](../relational-databases/policy-based-management/monitor-and-enforce-best-practices-by-using-policy-based-management.md)  
  
  