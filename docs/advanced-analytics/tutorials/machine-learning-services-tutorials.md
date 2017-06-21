---
title: "SQL Server Machine Learning Tutorials | Microsoft Docs"
ms.custom: 
  - "SQL2016_New_Updated"
ms.date: "06/20/2017"
ms.prod: "sql-server-2016"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "r-services"
ms.tgt_pltfrm: ""
ms.topic: "article"
applies_to: 
  - "SQL Server 2016"
dev_langs: 
  - "Python"
ms.assetid: 5ccc75f6-6703-47d9-b879-9a740569b45e
caps.latest.revision: 32
author: "jeannt"
ms.author: "jeannt"
manager: "jhubbard"
---
# SQL Server Machine Learning Tutorials

This article provides a comprehensive list of the tutorials, demos, and sample applications that use R or Python with SQL Server 2016 or SQL Server 2017. Start here to learn how to run R or Python from T-SQL, use remote and local compute contexts, and optimize your R and Python code for a SQL production environment.

## Start Here

+ [**Python tutorials**](#bkmk_pythontutorials)

+ [**R tutorials**](#bkmk_rtutorials)

### Related resources

+ [Samples](#bkmk_samples) are real-world scenarios for embedding machine learning in applications. All samples include code that you can download, modify, and use in production.

+ [Solutions](#bkmk_solutions) from the Microsoft Data Science team demonstrate end-to-end scenarios. Most are designed to run either in SQL Server or in a cloud environment such as Azure machine Learning.

For more information about requirements and how to get set up, see [Prerequisites](#bkmk_prerequisites).

## <a name="bkmk_prerequisites"></a>Prerequisites

To use these tutorials and samples, you must have installed one of the following server products:

+ SQL Server 2016 R Services (In-Database)
  
  Supports R. Be sure to install the machine learning features, and then enable external scripting.

+ SQL Server 2017 Machine Learning Services (In-Database)
  
  Supports either R or Python. You must select the machine learning feature and the language to install, and then enable external scripting.

For more information about setup, see [Prerequisites](#bkmk_Prerequisites).

> [!NOTE]
>
> Support for Python is a new feature in SQL Server 2017 (CTP 2.0). Although the feature is in pre-release and not supported for production environments, we invite you to try it out and send feedback.

## <a name ="bkmk_samples"></a>SQL Server Product Samples

These samples and demos provided by the SQL Server and R Server development team highlight ways that you can use embedded analytics in real-world applications.

+ [Build a predictive model using R and SQL Server](https://microsoft.github.io/sql-ml-tutorials/R/rentalprediction)

  Learn how a ski rental business might use machine learning to predict future rentals, which helps the business plan and staff to meet future demand.

+ [Build a predictive model using Python and SQL Server](https://microsoft.github.io/sql-ml-tutorials/python/rentalprediction/)

   Build the ski rental analysis application using Python, to help plan for future demand.

+ [Clustering in SQL Server R Services](https://microsoft.github.io/sql-ml-tutorials/R/customerclustering/)

  Use unsupervised learning to segment customers based on sales data. 

## Learn RevoScaleR

Wondering what RevoScaleR offers that R doesn't? See these tutorials:

+ [Explore R and Scale R in 25 Short Functions](https://msdn.microsoft.com/microsoft-r/microsoft-r-getting-started-tutorial)

  This R Server tutorial demonstrates how you can write R code once and deploy anywhere, using RevoScaleR data sources and remote compute contexts.

+ [How-to guides for data analysis in Microsoft R](https://msdn.microsoft.com/microsoft-r/data-analysis-in-microsoft-r)

  Learn how to use RevoScaleR for advanced modeling and data transformation, and optimize for different compute contexts.

## <a name="bkmk_solutions"></a>Customizable End-to-End Solutions

The Microsoft Data Science Team has provided solution templates that can be used to jump-start solutions for common scenarios. All T-SQL and R code is provided, along with instructions on how to train and deploy a model for scoring using SQL Server stored procedures.

+ [Fraud detection](https://gallery.cortanaanalytics.com/Tutorial/Online-Fraud-Detection-Template-with-SQL-Server-R-Services-1)
+ [Custom churn prediction](https://gallery.cortanaanalytics.com/Tutorial/Customer-Churn-Prediction-Template-with-SQL-Server-R-Services-1)
+ [Predictive maintenance](https://gallery.cortanaanalytics.com/Tutorial/Predictive-Maintenance-Template-with-SQL-Server-R-Services-1)
+ [Predict hospital length of stay](https://gallery.cortanaintelligence.com/Solution/Predicting-Length-of-Stay-in-Hospitals-1)

For more information, see [Machine Learning Templates with SQL Server 2016 R Services](https://blogs.technet.microsoft.com/machinelearning/2016/03/23/machine-learning-templates-with-sql-server-2016-r-services/).

## Resources and Reading

+ Want to know the real story behind R Services? Read this article from the development and PM team: [Why did we build it?](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2017/01/10/sql-server-r-services-why-did-we-build-it/)


## <a name="bkmk_Prerequisites"></a>Prerequisites

To run these tutorials, you must download and install the SQL Server machine learning components, as described here:

+ [Set up SQL Server R Services](../r/set-up-sql-server-r-services-in-database.md)
+ [Set up SQL Server Python Services](../python/setup-python-machine-learning-services.md)

With SQL Server 2017 CTP 2.0, you can install either R or Python, or both. Otherwise the overall setup process, architecture, and requirements are the same.

After running SQL Server setup, don't forget these important steps:

+ Enable the external script execution feature by running `sp_configure 'enable external script', 1`
+ Restart the server
+ Ensure that the service that calls the external runtime has necessary permissions
+ Ensure that your SQL login or Windows user account has necessary permissions to connect to the server, to read data, and to create any database objects required by the sample

If you run into trouble, see this article for some common issues: [Upgrade and Installation of SQL Server R Services](../../advanced-analytics/r-services/upgrade-and-installation-faq-sql-server-r-services.md)

> [!NOTE]
> You cannot run these tutorials using just the standard R or Python tools. Both your development environment and the SQL Server computer with machine learning must have the R or Python libraries provided by Microsoft.
