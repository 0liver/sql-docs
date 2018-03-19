---
title: "What&#39;s new in SQL Server Machine Learning Services | Microsoft Docs"
ms.date: "03/08/2018"
ms.prod: "machine-learning-services"
ms.prod_service: "machine-learning-services"
ms.service: ""
ms.component: ""
ms.reviewer: ""
ms.suite: "sql"
ms.technology: 
  
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: 
caps.latest.revision: 36
author: "jeannt"
ms.author: "jeannt"
manager: "cgronlun"
ms.workload: "On Demand"
---
# What's new in SQL Server Machine Learning Services 
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

Machine learning capabilities are added to SQL Server in each release as we continue to expand, extend, and deepen the integration between the data platform and the data science, analytics, and supervised learning you want to implement over your data. 

## New in SQL Server 2017

This release added Python support and industry-leading machine learning algorithms. Renamed to reflect the new scope, SQL Server 2017 marked the introduction of SQL Server Machine Learning Services (In-Database), with language support for both Python and R. 

This release also introduced SQL Server Machine Learning Server (Standalone), fully independent of SQL Server, for R and Python workloads that you want to run on a dedicated system. With the standalone server, you can distribute and scale R or Python code without using SQL Server.

| Release | Feature update |
|---------|---------------|
| CU 4 | Bug fixes and package refresh, but no new feature announcements. |
| CU 3 | 1. Python model serialization in revoscalepy, using the [rx_serialize_model function](https://docs.microsoft.com/machine-learning-server/python-reference/revoscalepy/rx-serialize-model).<br/><br/>2. [Native scoring](sql-native-scoring.md) plus enhancements to [Realtime scoring](real-time-scoring.md). With in-database scoring, throughput is a million rows per second using R models. In this update, realtime scoring and native scoring offer better performance in single-row and batch scoring. Native scoring uses a T-SQL function for fast scoring that can be run on any edition of SQL Server, even on Linux. The function requires no installation of R or extra configuration. This means you can train a model elsewhere, save it in SQL Server, and then perform scoring without ever calling R. For more information on scoring methodologies, see [How to perform realtime scoring or native scoring](r/how-to-do-realtime-scoring.md). |
| CU 2 | Bug fixes and package refresh, but no new feature announcements. |
| CU 1 | 1. In revoscalepy, adds rx_create_col_info for returning schema information from a SQL Server data source, similar to [rxCreateColInfo](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/rxcreatecolinfo) for  R. <br/><br/>2. Enhancements to [rx_exec](https://docs.microsoft.com/machine-learning-server/python-reference/revoscalepy/rx-exec) to support parallel scenarios using the `RxLocalParallel` compute context.|
| Initial release |[**Python support for in-database analytics**](https://blogs.technet.microsoft.com/dataplatforminsider/2017/04/19/python-in-sql-server-2017-enhanced-in-database-machine-learning/) <br/><br/>The [revoscalepy](python/what-is-revoscalepy.md) package is the Python-equivalent of RevoScaleR. You can create Python models for linear and logistic regressions, decision trees, boosted trees, and random forests, all parallelizable, and capable of being run in remote compute contexts. This package supports use of multiple data sources and remote compute contexts. The data scientist or developer can execute Python code on a remote SQL Server, to explore data or build models without moving data. <br/><br/>The [microsoftml](https://docs.microsoft.com/machine-learning-server/python-reference/microsoftml/microsoftml-package) package is the Python-equivalent of the MicrosoftML R package.<br/><br/>T-SQL and Python integration through [sp_execute_external_script](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql). You can call any Python code using this stored procedure. This secure infrastructure enables enterprise-grade deployment of Python models and scripts that can be called from an application using a simple stored procedure. Additional performance gains are achieved by streaming data from SQL to Python processes and MPI ring parallelization. |
| Initial release | [**MicrosoftML (R)**](using-the-microsoftml-package.md) contains state-of-the-art machine learning algorithms and data transformation that can be scaled or run in remote compute contexts. Algorithms include customizable deep neural networks, fast decision trees and decision forests, linear regression, and logistic regression. |
| Initial release | [**Pre-trained models**](r/install-pretrained-models-sql-server.md) for image recognition and positive-negative sentiment analysis. Use these models to generate predictions on your own data. |
| Initial release | [**Package management**](r/r-package-management-for-sql-server-r-services.md), including the following highlights: database roles to help the DBA manage packages and assign permissions to install packages, [CREATE EXTERNAL LIBRARY](https://docs.microsoft.com/sql/t-sql/statements/create-external-library-transact-sql) statement in T-SQL to help DBAs manage packages without needing to know R, and a rich set of R functions in [RevoScaleR](r/use-revoscaler-to-manage-r-packages.md) to help install, remove, or list packages owned by users. |
| Initial release | [**Operationalization through mrsdeploy**](https://docs.microsoft.com/machine-learning-server/r-reference/mrsdeploy/mrsdeploy-package) for deploying and hosting R script as a web service. Applies to R script only (no Python equivalent). Intended for the (Standalone) server option to avoid resource competition with other SQL Server operations. |


## New in SQL Server 2016

This release introduced machine learning capabilities into SQL Server through **SQL Server 2016 R Services**, an in-database analytics engine for processing R script on resident data within a database engine instance.

Additionally, **SQL Server 2016 R Server (Standalone)** was released as a way to install R Server on a Windows server. Initially, SQL Server Setup provided the only way to install R Server for Windows. In later releases, developers and data scientists who wanted R Server on Windows could use another standalone installer to achieve the same goal. The standalone server in SQL Server is functionally equivalent to the standalone server product, [Microsoft R Server for Windows](https://docs.microsoft.com/machine-learning-server/install/r-server-install-windows).

| Release |Feature update |
|---------|----------------|
| CU | [Realtime scoring](real-time-scoring.md). Realtime scoring relies on native C++ libraries to read a model stored in an optimized binary format, and then generate predictions without having to call the R runtime. This makes scoring operations much faster. With realtime scoring, you can run a stored procedure or perform realtime scoring from R code. Realtime scoring is also available for SQL Server 2016, if the instance is upgraded to the latest release of [!INCLUDE[rsql-platform-md](../includes/rsql-platform-md.md)]. |
| Initial release | R packages and interpreter for calling R functions in T-SQL, and vice versa.<br/>RevoScaleR functions provide R analytics at scale by chunking data into component parts, coordinating and managing distributed processing, and aggregating results. In SQL Server 2016 R Services (In-Database), the RevoScaleR engine is integrated with a database engine instance, brining data and analytics together in the same processing context. |

## Linux support roadmap

Machine learning using R or Python in-database is not currently supported in SQL Server on Linux. Look for announcements in a later release.

However, on Linux you can perform [native scoring](sql-native-scoring.md) using the T-SQL PREDICT function. Native scoring lets you score from a pretrained model very fast, without calling or even requiring an R runtime. This means you can use SQL Server on Linux to generate predictions very fast, to serve client applications.

### Next steps

+ [Install SQL Server Machine Learning Services](../advanced-analytics/python/setup-python-machine-learning-services.md)
+ [Machine learning tutorials and samples](tutorials/machine-learning-services-tutorials.md)
