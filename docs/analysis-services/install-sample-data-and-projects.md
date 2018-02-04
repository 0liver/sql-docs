---
title: "Install Sample Data and Projects | Microsoft Docs"
ms.custom: ""
ms.date: "02/02/2018"
ms.prod: analysis-services
ms.prod_service: "analysis-services, azure-analysis-services"
ms.service: ""
ms.component: ""
ms.reviewer: ""
ms.suite: "pro-bi"
ms.technology: 
  
ms.tgt_pltfrm: ""
ms.topic: "get-started-article"
applies_to: 
  - "SQL Server 2016"
ms.assetid: fc475b25-cbb2-408a-901f-9299299538c5
caps.latest.revision: 16
author: "Minewiskan"
ms.author: "owend"
manager: "kfile"
ms.workload: "Active"
---
# Install sample data and multidimensional projects 
[!INCLUDE[ssas-appliesto-sqlas-all](../includes/ssas-appliesto-sqlas-all.md)]

Use the instructions and links provided in this topic to install the data and project files used in the Analysis Services Tutorials.  If you are completing the multidimensional tutorial, you only need to install the sample projects if you want compare a fully completed project with the one you create in the tutorial.
  
## Step 1: Install SQL Server Software  
The lessons in this tutorial assume that you have the following software installed. You can install all of the features on a single computer. To install these features, run SQL Server Setup and select them from the Feature Selection page.  
  
-   Database Engine  
  
-   Analysis Services  
  
    Analysis Services is available in these editions only: Evaluation, Enterprise, Business Intelligence, Standard.  
  
    By default, Analysis Services 2016 and later is installed as a tabular instance, which you can override by choosing Multidimensional Server Mode in the server configuration page of the Installation Wizard. If you want to run both server modes, rerun SQL Server Setup on the same computer to install a second instance of Analysis Services in the other mode.  
  
-   [SQL Server Management Studio](../ssms/download-sql-server-management-studio-ssms.md)  
  
Optionally, consider installing Excel to browse your multidimensional data as you proceed through the tutorial. Installing Excel enables the **Analyze in Excel** feature that starts Excel using a PivotTable field list that is connected to the cube you are building. Using Excel to browse data is recommended because you can quickly build a pivot report that lets you interact with the data.  
  
Alternatively, you can browse data using the built-in MDX query designer that is built into [!INCLUDE[ssBIDevStudioFull](../includes/ssbidevstudiofull-md.md)]. The query designer returns the same data, except the data is presented as a flat rowset.  
  
## Step 2: Download SQL Server Data Tools for Visual Studio 
In this release, SQL Server Data Tools is downloaded and installed separately from other SQL Server features. The designers and project templates used to create BI models and reports are included in SSDT for Visual Studio 2015 or as [Nuget packages](https://marketplace.visualstudio.com/items?itemName=ProBITools.MicrosoftAnalysisServicesModelingProjects) for Visual Studio 2017.  
  
-   [Download SQL Server Data Tools](http://go.microsoft.com/fwlink/?LinkID=827542). The file is saved to the Downloads folder. Run setup to install the tool.  
  
## Step 3: Install Databases  
An Analysis Services multidimensional model uses transactional data that you import from a relational database management system. For the purposes of this tutorial, you will use the following relational database as your data source.  
  
-   **AdventureWorksDW 2012 or later** – This is a relational data warehouse that runs on a Database Engine instance. It provides the original data that will be used by the Analysis Services databases and projects that you build and deploy throughout the tutorial.  
  
    You can use this sample database with [!INCLUDE[ssSQL11](../includes/sssql11-md.md)] and later. In-general, you should use the sample database version matching your database engine version.
  
To install the database, do the following:  
  
1.  Download an [AdventureWorkDW](https://github.com/Microsoft/sql-server-samples/releases/tag/adventureworks) database backup from GitHub.  
  
2.  Copy the backup file to the data directory of the local SQL Server Database Engine instance.
  
3.  Start SQL Server Management Studio and connect to the Database Engine instance.  
  
4.  Restore the database.  
  
## Step 4: Grant Database Permissions  
The sample projects use data source impersonation settings that specify the security context under which data is imported or processed. By default, the impersonation settings specify the Analysis Services service account for accessing the data. To use this default setting, you must ensure that the service account under which Analysis Services runs has data reader permissions on the **AdventureWorksDW2014** database.  
  
> [!NOTE]  
> For learning purposes, it is recommended that you use the default service account impersonation option and grant data reader permissions to the service account in SQL Server. Although other impersonation options are available, not all of them are suitable for processing operations. Specifically, the option for using the credentials of the current user is not supported for processing.  
  
1.  Determine the service account. You can use SQL Server Configuration Manager or the Services console application to view account information. If you installed Analysis Services as the default instance, using the default account, the service is running as **NT Service\MSSQLServerOLAPService**.  
  
2.  In Management Studio, connect to the database engine instance.  
  
3.  Expand the Security folder, right-click Logins and select **New Login**.  
  
4.  On the General page, in Login name, type **NT Service\MSSQLServerOLAPService** (or whatever account the service is running as).  
  
5.  Click **User Mapping**.  
  
6.  Select the checkbox next to the **AdventureWorksDW2014** database. Role membership should automatically include **db_datareader** and **public**. Click **OK** to accept the defaults.  
  
## Step 5: Install Projects  

The sample projects are only necessary to compare with what you create in the multidimensional modeling tutorial. These are not necessary to complete the tutorial.
  
1.  Download the [adventure-works-multidimensional-model-project.zip](https://github.com/Microsoft/sql-server-samples/releases/tag/adventureworks-analysis-services) from the Adventure Works for Analysis Services samples page on GitHub.  
  
    This project works for SSAS 2014 and later.  
  
2.  Move the .zip file to a folder just below the root drive (for example, C:\Tutorial). This step mitigates the “Path too long” error that sometimes occurs if you attempt to unzip the files in the Downloads folder.  
  
3.  Unzip the sample projects: right-click on the file and select **Extract All**. 
  
4.  Remove the read-only permissions on these files. Right-click the parent folder,  select **Properties**, and clear the checkbox for **Read-only**. Click **OK**. Apply the changes to this folder, subfolders, and files.  

5.  Open the solution (.sln) file in [!INCLUDE[ssBIDevStudioFull](../includes/ssbidevstudiofull-md.md)].  
  
6.  Deploy the solution to verify that database permissions and server location information is set up correctly.  
  
    If Analysis Services and the Database Engine are installed as the default instance (MSSQLServer) and all software is running on the same computer, you can click **Deploy Solution** on the Build menu to build and deploy the sample project to the local Analysis Services instance. During deployment, data will be processed (or imported) from the **AdventureWorksDW** database on the local Database Engine instance. A new Analysis Services database will be created on the Analysis Services instance that contains the data retrieved from the Database Engine.  
  
    If you encounter errors, review the previous steps on setting up database permissions. Additionally, you might also need to change server names. The default server name is localhost. If your servers are installed on remote computers or as named instances, you must override the default to use a server name that is valid for your installation. Furthermore, if the servers are on remote computers, you might need to configure Windows Firewall to allow access to the servers.  
  
    The server name for connecting to the database engine is specified in the Data Source object of the multidimensional solution (Adventure Works Tutorial), visible in Solution Explorer.  
  
    The server name for connecting to Analysis Services is specified in the Deployment tab of the Property Pages of the project, also visible in Solution Explorer.  
  
7.  In SQL Server Management Studio, connect to Analysis Services. Verify the database named **Analysis Services Tutorial** is running on the server.  
  
## Next Step  
You are now ready to use the tutorial. For more information about how to get started, see [Multidimensional Modeling &#40;Adventure Works Tutorial&#41;](../analysis-services/multidimensional-modeling-adventure-works-tutorial.md).  
  
## See Also  
[Configure the Windows Firewall to Allow Analysis Services Access](../analysis-services/instances/configure-the-windows-firewall-to-allow-analysis-services-access.md)  
[Configure the Windows Firewall to Allow SQL Server Access](../sql-server/install/configure-the-windows-firewall-to-allow-sql-server-access.md)  
  
  