---
title: "Intelligent query processing"
description: "Intelligent query processing features to improve query performance in SQL Server, Azure SQL Managed Instance, and Azure SQL Database."
ms.prod: sql
ms.prod_service: "database-engine, sql-database"
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords: 
author: "MikeRayMSFT"
ms.author: "mikeray"
ms.reviewer: "wiassaf"
ms.custom:
- seo-dt-2019
- event-tier1-build-2022
ms.date: 07/21/2022
monikerRange: "=azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current"
---

# Intelligent query processing in SQL databases

[!INCLUDE [SQL Server Azure SQL Database Azure SQL Managed Instance](../../includes/applies-to-version/sql-asdb-asdbmi.md)]

The intelligent query processing (IQP) feature family includes features with broad impact that improve the performance of existing workloads with minimal implementation effort to adopt. The following graphic details the family of IQP features and when they were first introduced for SQL Server. All IQP features are available in [!INCLUDE[ssazuremi_md](../../includes/ssazuremi_md.md)] and [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]. Some features depend on the database's compatibility level. 

![A diagram of the Intelligent Query Processing family of features and when they were first introduced to SQL Server.](./media/iqp-feature-family.svg)

Watch this 6-minute video for an overview of intelligent query processing:

> [!VIDEO https://channel9.msdn.com/Shows/Data-Exposed/Overview-Intelligent-Query-processing-in-SQL-Server-2019/player?WT.mc_id=dataexposed-c9-niner]

For demos and sample code of intelligent query processing (IQP) features on GitHub, visit [https://aka.ms/IQPDemos](https://aka.ms/IQPDemos).

You can make workloads automatically eligible for intelligent query processing by enabling the applicable database compatibility level for the database. You can set this using [!INCLUDE[tsql](../../includes/tsql-md.md)]. For example:

```sql
ALTER DATABASE [WideWorldImportersDW] SET COMPATIBILITY_LEVEL = 150;
```

The following table details all intelligent query processing features, along with any requirement they have for database compatibility level.

## <a id="sql2022"></a> IQP features for [!INCLUDE[sssql22-md](../../includes/sssql22-md.md)], [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)], [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)]

| **IQP Feature** | **Supported in [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] and [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)]** | **Supported in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]** |**Description** |
| ---------------- | ------- | ------- | ---------------- |
| [Adaptive Joins (Batch Mode)](intelligent-query-processing-details.md#batch-mode-adaptive-joins) | Yes, starting with database compatibility level 140| Yes, starting in [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] with database compatibility level 140|Adaptive joins dynamically select a join type during runtime based on actual input rows.|
| [Approximate Count Distinct](intelligent-query-processing-details.md#approximate-query-processing) | Yes| Yes, starting in [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)]|Provide approximate COUNT DISTINCT for big data scenarios with the benefit of high performance and a low memory footprint. |
| [Batch Mode on Rowstore](intelligent-query-processing-details.md#batch-mode-on-rowstore) | Yes, starting with database compatibility level 150| Yes, starting in [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] with compatibility level 150|Provide batch mode for CPU-bound relational DW workloads without requiring columnstore indexes.  |
| [Degrees of Parallelism (DOP) feedback](intelligent-query-processing-details.md#dop-feedback) | Yes, starting with database compatibility level 160| Yes, starting in [!INCLUDE[sql-server-2022](../../includes/sssql22-md.md)] with compatibility level 160|Automatically adjusts degree of parallelism for repeating queries to optimize for workloads where inefficient parallelism can cause performance issues. Requires Query Store to be enabled.|
| [Interleaved Execution](intelligent-query-processing-details.md#interleaved-execution-for-mstvfs) | Yes, starting with database compatibility level 140| Yes, starting in [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] with database compatibility level 140|Uses the actual cardinality of the multi-statement table valued function encountered on first compilation instead of a fixed guess.|
| [Memory Grant Feedback (Batch Mode)](intelligent-query-processing-details.md#batch-mode-memory-grant-feedback) | Yes, starting with database compatibility level 140| Yes, starting in [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] with database compatibility level 140|If a batch mode query has operations that spill to disk, add more memory for consecutive executions. If a query wastes > 50% of the memory allocated to it, reduce the memory grant size for consecutive executions.|
| [Memory Grant Feedback (Row Mode)](intelligent-query-processing-details.md#row-mode-memory-grant-feedback) | Yes, starting with database compatibility level 150| Yes, starting in [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] with database compatibility level 150|If a row mode query has operations that spill to disk, add more memory for consecutive executions. If a query wastes > 50% of the memory allocated to it, reduce the memory grant size for consecutive executions.|
| [Memory Grant Feedback (Percentile and Persistence)](intelligent-query-processing-details.md#percentile-and-persistence-mode-memory-grant-feedback) | Yes, starting with database compatibility level 150| Yes, starting with [!INCLUDE[sql-server-2022](../../includes/sssql22-md.md)]) with database compatibility level 140 | Addresses existing limitations of memory grant feedback in a non-intrusive way.  |
| [Scalar UDF Inlining](intelligent-query-processing-details.md#scalar-udf-inlining) | Yes, starting with database compatibility level 150 | Yes, starting in [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] with database compatibility level 150|Scalar UDFs are transformed into equivalent relational expressions that are "inlined" into the calling query, often resulting in significant performance gains.|
| [Parameter Sensitivity Plan Optimization](intelligent-query-processing-details.md#parameter-sensitivity-plan-optimization) | Yes, starting with database compatibility level 150 | Yes, (Starting in [!INCLUDE[sql-server-2022](../../includes/sssql22-md.md)]) with database compatibility level 150 | Parameter Sensitivity Plan Optimization addresses the scenario where a single cached plan for a parameterized query is not optimal for all possible incoming parameter values, for example non-uniform data distributions. |
| [Table Variable Deferred Compilation](intelligent-query-processing-details.md#table-variable-deferred-compilation) | Yes, starting with database compatibility level 150 | Yes, starting in [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] with database compatibility level 150 | Uses the actual cardinality of the table variable encountered on first compilation instead of a fixed guess.|

## <a id="sql2019"></a> IQP features for [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)]

| **IQP Feature** | **Supported in [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)]** |**Description** |
| ---------------- | ------- | ------- | ---------------- |
| [Adaptive Joins (Batch Mode)](intelligent-query-processing-details.md#batch-mode-adaptive-joins) | Yes, starting in [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] with database compatibility level 140|Adaptive joins dynamically select a join type during runtime based on actual input rows.|
| [Approximate Count Distinct](intelligent-query-processing-details.md#approximate-query-processing) | Yes| Provide approximate COUNT DISTINCT for big data scenarios with the benefit of high performance and a low memory footprint. |
| [Batch Mode on Rowstore](intelligent-query-processing-details.md#batch-mode-on-rowstore) | Yes, starting with database compatibility level 150| Provide batch mode for CPU-bound relational DW workloads without requiring columnstore indexes.  |
| [Interleaved Execution](intelligent-query-processing-details.md#interleaved-execution-for-mstvfs) | Yes, starting with database compatibility level 140| Use the actual cardinality of the multi-statement table valued function encountered on first compilation instead of a fixed guess.|
| [Memory Grant Feedback (Batch Mode)](intelligent-query-processing-details.md#batch-mode-memory-grant-feedback) | Yes, starting with database compatibility level 140| If a batch mode query has operations that spill to disk, add more memory for consecutive executions. If a query wastes > 50% of the memory allocated to it, reduce the memory grant size for consecutive executions.|
| [Memory Grant Feedback (Row Mode)](intelligent-query-processing-details.md#row-mode-memory-grant-feedback) | Yes, starting with database compatibility level 150| If a row mode query has operations that spill to disk, add more memory for consecutive executions. If a query wastes > 50% of the memory allocated to it, reduce the memory grant size for consecutive executions.|
| [Memory Grant Feedback (Percentile and Persistence)](intelligent-query-processing-details.md#percentile-and-persistence-mode-memory-grant-feedback) | Yes, introduced in [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] but applies to database compatibility level 140 and higher | Addresses existing limitations of memory grant feedback in a non-intrusive way. |
| [Scalar UDF Inlining](intelligent-query-processing-details.md#scalar-udf-inlining) | Yes, starting with database compatibility level 150 | Scalar UDFs are transformed into equivalent relational expressions that are "inlined" into the calling query, often resulting in significant performance gains.|
| [Table Variable Deferred Compilation](intelligent-query-processing-details.md#table-variable-deferred-compilation) | Yes, starting with database compatibility level 150 | Use the actual cardinality of the table variable encountered on first compilation instead of a fixed guess.|

## <a id="sql2019"></a> IQP features for [!INCLUDE[sssql17-md](../../includes/sssql17-md.md)]

| **IQP Feature** | **Supported in [!INCLUDE[sssql17-md](../../includes/sssql17-md.md)]** |**Description** |
| ---------------- | ------- | ------- | ---------------- |
| [Adaptive Joins (Batch Mode)](intelligent-query-processing-details.md#batch-mode-adaptive-joins) | Yes, starting in [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] with database compatibility level 140|Adaptive joins dynamically select a join type during runtime based on actual input rows.|
| [Approximate Count Distinct](intelligent-query-processing-details.md#approximate-query-processing) | Yes| Provide approximate COUNT DISTINCT for big data scenarios with the benefit of high performance and a low memory footprint. |
| [Interleaved Execution](intelligent-query-processing-details.md#interleaved-execution-for-mstvfs) | Yes, starting with database compatibility level 140| Use the actual cardinality of the multi-statement table valued function encountered on first compilation instead of a fixed guess.|
| [Memory Grant Feedback (Batch Mode)](intelligent-query-processing-details.md#batch-mode-memory-grant-feedback) | Yes, starting with database compatibility level 140| If a batch mode query has operations that spill to disk, add more memory for consecutive executions. If a query wastes > 50% of the memory allocated to it, reduce the memory grant size for consecutive executions.|
| [Memory Grant Feedback (Percentile and Persistence)](intelligent-query-processing-details.md#percentile-and-persistence-mode-memory-grant-feedback) | Yes, introduced in [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] but applies to database compatibility level 140 and higher | Addresses existing limitations of memory grant feedback in a non-intrusive way. |

For complete details on all IQP features, including release notes and more in-depth descriptions, see [Intelligent query processing (IQP) feature details](intelligent-query-processing-details.md).

## See also

- [Joins](../../relational-databases/performance/joins.md)
- [Execution modes](../../relational-databases/query-processing-architecture-guide.md#execution-modes)
- [Query processing architecture guide](../../relational-databases/query-processing-architecture-guide.md)
- [Showplan logical and physical operators reference](../../relational-databases/showplan-logical-and-physical-operators-reference.md)
- [What's new in SQL Server 2017](../../sql-server/what-s-new-in-sql-server-2017.md)
- [What's new in SQL Server 2019](../../sql-server/what-s-new-in-sql-server-2019.md)
- [What's new in SQL Server 2022](../../sql-server/what-s-new-in-sql-server-2022.md)

## Next steps

- [Parameter Sensitivity Plan Optimization](parameter-sensitivity-plan-optimization.md)
- [Demonstrating Intelligent Query Processing](https://github.com/Microsoft/sql-server-samples/tree/master/samples/features/intelligent-query-processing)
- [Constant Folding and Expression Evaluation](../query-processing-architecture-guide.md#constant-folding-and-expression-evaluation)
- [Intelligent query processing demos on GitHub](https://aka.ms/IQPDemos)
- [Performance Center for SQL Server Database Engine and Azure SQL Database](../../relational-databases/performance/performance-center-for-sql-server-database-engine-and-azure-sql-database.md)
- [Monitor performance by using the Query Store](monitoring-performance-by-using-the-query-store.md)
- [Best practices with Query Store](best-practice-with-the-query-store.md)