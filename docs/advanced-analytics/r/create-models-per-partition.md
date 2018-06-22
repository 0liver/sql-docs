---
title: Create partition-based models (SQL Server Machine Learning Services) | Microsoft Docs
ms.custom: sqlseattle
ms.prod: sql
ms.technology: machine-learning
  
ms.date: 06/22/2018
ms.topic: conceptual
ms.author: heidist
author: HeidiSteen
manager: cgronlun
---
---
# Create partition-based models in SQL Server Machine Learning Services
[!INCLUDE[appliesto-ssvnex-xxxx-xxxx-xxx-md-winonly](../../includes/tsql-appliesto-ssvnext-xxxx-xxxx-xxx.md)]

**(Not for production workloads)**

One of the more common approaches for executing R or Python code on SQL data is providing script as an input parameter to the [sp_execute_external_script](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql) stored procedure. In this CTP release, SQL Server vNext adds new parameters to `sp_execute_external_script` to process partitions in parallel:

+ **input_data_1_partition_by_columns**, specifying which columns to partition by
+ **input_data_1_order_by_columns**, specifying which columns to order by

Partitions are an organizational mechanism for stratified data that naturally segments into a given classification scheme. Common examples include partitioning by geographic region, by date and time, by age or gender, and so forth. Given the existence of partitioned data, you might want to execute script over the entire data set, with the ability to model, train, and score partitions that remain intact over all these operations. Calling `sp_execute_external_script` with the new parameters allows you to do just that.

When the scenario is training, one advantage is that any arbitrary training script, even those using non-Microsoft-rx algorithms, can be parallelized by also using the @parallel parameter. Typically, you would have to use RevoScaleR algorithms (with the rx prefix) to obtain parallelism in training scenarios in SQL Server. But with the new parameter, you can parallelize a script that calls functions not specifically engineered with that capability.

> [!Note]
> If the input query can be parallelized, set `@parallel=1` as part of your arguments to `sp_execute_external_script`.

## R Example
 
This example uses the [NYC Taxi and Limousine Commission] (http://www.nyc.gov/html/tlc/html/about/trip_record_data.shtml) public data set. Data is stored in five partitions, resulting in five individual models. 
 
INSERT TRAINING SCRIPT HERE
 
In the same way, partition by can be used to for scoring. The following sample contains an R script that will score using the correct model for the partition it is currently processing.
 
INSERT SCORING SCRIPT HERE

