---
title: "SQL Server to SQL Database - migration overview"
description: Learn about the different tools and options available to migrate your SQL Server databases to Azure SQL Database.
ms.service: sql-database
ms.subservice: migration
ms.custom: 
ms.devlang: 
ms.topic: conceptual
author: MashaMSFT
ms.author: mathoma
ms.reviewer: mokabiru
ms.date: 08/25/2020
---
# Migration overview: SQL Server to SQL Database
[!INCLUDE[appliesto--sqldb](../../includes/appliesto-sqldb.md)]

Learn about different migration options and considerations to migrate your SQL Server to Azure SQL Database. 

You can migrate SQL Server running on-premises or on: 

- SQL Server on Virtual Machines  
- Amazon Web Services (AWS) EC2 
- Amazon Relational Database Service (AWS RDS) 
- Compute Engine (Google Cloud Platform - GCP)  
- Cloud SQL for SQL Server (Google Cloud Platform – GCP) 

For other scenarios, see the [Database Migration Guide](https://datamigration.microsoft.com/). 

## Overview
[Azure SQL Database](/azure/azure-sql/database/sql-database-paas-overview) is a recommended target option for SQL Server workloads that require a fully managed Platform as a Service (PaaS) that handles most of the database management functions with capabilities built in to provide high availability, intelligent query processing, scalability and performance to suit different types of applications. Azure SQL Database provides flexibility and choice with multiple [deployment models](/azure/azure-sql/database/sql-database-paas-overview#deployment-models) and [service tiers](/azure/azure-sql/database/service-tiers-vcore?tabs=azure-portal#service-tiers) that caters to different types of applications or workloads.

You can also leverage cost optimization and savings by migrating your SQL Server on-premises licenses to Azure SQL Database using the [Azure Hybrid Benefit](https://azure.microsoft.com/pricing/hybrid-benefit/) for SQL Server should you choose the [vCore-based purchasing model](https://docs.microsoft.com/azure/azure-sql/database/service-tiers-vcore?tabs=azure-portal).

## Considerations 

The key factors to consider when evaluating migration options depend on: 
- Number of servers and databases
- Size of databases
- Acceptable business downtime during the migration process 

Based on these factors, choose between an online and offline migration. During an offline migration, your application incurs downtime the entirety of the migration whereas for an online migration, application downtime is limited to the time required to cut over to SQL Database during the final step of the migration process. 

One of the key benefits of migrating your SQL Servers to SQL Database is that you can modernize your application by leveraging the capabilities of the PaaS Azure SQL Database and eliminate any dependency on technical components that are scoped at the instance level such as SQL Agent jobs.




### Choose an appropriate database
Some general guidelines to help you choose the right deployment model and service tier of Azure SQL Database are listed below.

Deployment models: Understand your application workload and the usage pattern to decide between a Single database and Elastic pool. 
- A [single database](/azure/azure-sql/database/single-database-overview) represents a fully managed database suitable for most modern cloud applications and microservices.
- An [Elastic pool](/azure/azure-sql/database/elastic-pool-overview) is a collection of single databases with a shared set of resources such as CPU or memory and suitable for combining databases in a pool with predictable usage patterns that can effectively share the same set of resources.

Purchasing models: Choose between the vCore-based purchasing model, the DTU-based purchasing model and Hyperscale. 
- The [vCore-based purchasing model](/azure/azure-sql/database/service-tiers-vcore?tabs=azure-portal) is the easiest to translate from on-premises SQL Server as it lets you choose the number of vCores for your Azure SQL database along with the option to apply Azure Hybrid Benefit for cost savings. 
- The [DTU model](/azure/azure-sql/database/service-tiers-dtu) is designed to abstract the underlying compute, memory and IO resources in order to provide a blended DTU. The DTU model automatically includes SQL Server license in its pricing and hence Azure Hybrid Benefit cannot be applied in this model.
- The [serverless model](/azure/azure-sql/database/serverless-tier-overview) is purpose built for workloads that require automatic scaling on-demand with compute resources billed per second of usage. The serverless compute tier also automatically pauses databases during inactive periods when only storage is billed, and automatically resumes databases when activity returns.

Service tiers: Choose between the three service tiers that are designed for different types of applications.
- [General Purpose / Standard service tier](/azure/azure-sql/database/service-tier-general-purpose) is designed for most database workloads that offers a balanced budget-oriented option with compute and storage suitable to deliver mid-lower tier applications. It has redundancy built in at the storage layer to recover from failures.
- [Business Critical / Premium service tier](/azure/azure-sql/database/service-tier-business-critical) is designed for high tier applications that require mission critical performance to handle high transaction rates and low latency IO, high level of resiliency with secondary replicas to failover during issues.
- [Hyperscale](/azure/azure-sql/database/service-tier-hyperscale) service tier is designed for very large databases that have growing data volumes and need the capabilities to automatically scale upto 100 TB database size.


You can choose compute and storage resources during deployment and then change them after using the  [Azure portal](https://review.docs.microsoft.com/en-us/azure/azure-sql/database/scale-resources)  without incurring downtime for your application.


## Migration options

The following table describes data migration options and corresponding recommendations: 


|Migration option  |When to use  |Description  |Considerations  |
|---------|---------|---------|---------|
|[Azure Database Migration Service - online](/azure/dms/tutorial-sql-server-azure-sql-online) </br>  (Recommended) | - Migrating single databases or multiple databases at scale.  </br> - Require minimal downtime. </br> - Database size is less than 250 GB </br> </br> Supported sources: </br> - SQL Server (2005 - 2019) on-premises or Azure VM </br> - AWS EC2 </br> - AWS RDS </br> - GCP Compute SQL Server VM  | First party Azure service that supports online migrations at scale. Downtime only required during cutover which is the time taken to migrate the final transaction set to the target Azure SQL Database. | - Migrations at scale can be automated via [PowerShell](/azure/dms/howto-sql-server-to-azure-sql-powershell). </br> - Application connection strings changes may be required.  </br> - Monitoring and cutover status is available in the online migration mode.  </br> - [Pre-requisites](azure/dms/tutorial-sql-server-azure-sql-online#prerequisites) include enabling CDC on source database tables if they don't have a primary key and configuring the distributor role for source SQL Server.|
|[Azure Database Migration Service - offline](/azure/dms/tutorial-sql-server-to-azure-sql) </br>  (Recommended) | - Migrating single databases or at scale. </br> - Can accommodate downtime during migration process. </br> </br> Supported sources: </br> - SQL Server (2005 - 2019) on-premises or Azure VM </br> - AWS EC2 </br> - AWS RDS </br> - GCP Compute SQL Server VM | First party Azure service that supports migration in the offline mode for applications that can afford downtime during the migration process. | - Migrations at scale can be automated via [PowerShell](/azure/dms/howto-sql-server-to-azure-sql-powershell). </br> - Time to complete migration is dependent on database size and the number of objects in the database. </br> - Requires the source database to set as Read-Only. |
|[Transactional replication](/azure/azure-sql/database/replication-to-sql-database) </br>  (Recommended) | - Migrating by continuously publishing changes from source database tables to target SQL Database database tables. </br> - Full or partial database migrations of selected tables (subset of database).  </br> </br> Supported sources: </br> - SQL Server (2012 - 2019) with some limitations </br> - AWS EC2  </br> - GCP Compute SQL Server VM | Replicate data from source SQL Server database table(s) to SQL Database by providing a publisher-subscriber type migration option while maintaining transactional consistency. | - Setup is relatively complex compared to other migration options.   </br> - Provides a continuous replication option to migrate data (without taking the databases offline).  </br> - It is possible to [monitor replication activity](/sql/relational-databases/replication/monitor/monitoring-replication).    |
|[Import Export Wizard / BACPAC](/azure/azure-sql/database/database-import?tabs=azure-powershell)| - Migrating individual Line-of-business application’s database(s). </br>- Suited for smaller databases.  </br>  - Does not require a separate migration service or tool. </br> </br> Supported sources: </br> - SQL Server (2005 - 2019) on-premises or Azure VM </br> - AWS EC2 </br> - AWS RDS </br> - GCP Compute SQL Server VM  | [BACPAC](/sql/relational-databases/data-tier-applications/data-tier-applications#bacpac) is a Windows file with a .bacpac extension that encapsulates a database's schema and data. BACPAC can be used to both export data from a source SQL Server and to import the data into Azure SQL Database.  | - Requires downtime as data needs to be exported at the source and imported at the destination.   </br> - The file formats and data types used in the export / import need to be consistent with table schemas to avoid truncation / data type mismatch errors.  </br> - Time taken to export a database with a large number of objects can be significantly higher.       |
|[Bulk copy](/sql/relational-databases/import-export/import-and-export-bulk-data-by-using-the-bcp-utility-sql-server)| - Migrating full or partial data migrations. </br> - Can accommodate downtime. </br> </br> Supported sources: </br> - SQL Server (2005 - 2019) on-premises or Azure VM </br> - AWS EC2 </br> - AWS RDS </br> - GCP Compute SQL Server VM   | The [bulk copy program (bcp) utility](/sql/tools/bcp-utility) copies data from an instance of SQL Server into a data file. Use the BCP utility to export the data from your source and import the data file into the target SQL Database. </br></br> For high-speed bulk copy operations to move data to Azure SQL Database, [Smart Bulk Copy tool](/samples/azure-samples/smartbulkcopy/smart-bulk-copy/) can be used to maximise transfer speed by leverging parallel copy tasks.| - Requires downtime for exporting data from source and importing into target. </br> - The file formats and data types used in the export / import need to be consistent with table schemas. |
|[Azure Data Factory (ADF)](/azure/data-factory/connector-azure-sql-database)| - Migrating and/or transforming data from source SQL Server database(s). </br> - Merging data from multiple sources of data to Azure SQL Database typically for Business Intelligence (BI) workloads.  </br> </br> ADF supports a wide range of [connectors](/azure/data-factory/connector-overview) to move data from SQL Server sources to SQL Database | The [Copy activity](/azure/data-factory/copy-activity-overview) in Azure Data Factory  migrates data from source SQL Server database(s) to SQL Database using built-in connectors and an [Integration Runtime](/azure/data-factory/concepts-integration-runtime).|  - Requires creating data movement pipelines in ADF to move data from source to destination.   </br> - [Cost](https://azure.microsoft.com/pricing/details/data-factory/data-pipeline/) is an important consideration and is based on the pipeline triggers, activity runs, duration of data movement, etc. |

## Feature interoperability 

There are additional considerations when migrating workloads that rely on other SQL Server features. 

### SQL Server Integration Services 

SSIS packages can be migrated to Azure by redeploying the packages to Azure-SSIS runtime in Azure Data Factory. Azure Data Factory [supports migration of SSIS packages](/azure/data-factory/scenario-ssis-migration-overview#azure-sql-database-as-database-workload-destination) by providing a runtime that is purpose built to execute SSIS packages in Azure. Alternatively, you can also re-write the SSIS ETL logic natively in ADF using [Dataflows](/azure/data-factory/concepts-data-flow-overview).


### SQL Server Reporting Services 

SQL Server Reporting Services (SSRS) reports can be migrated to paginated reports in Power BI. Use the [RDL Migration Tool](https://github.com/microsoft/RdlMigration) to help prepare, and migrate your reports. This tool was developed by Microsoft to help customers migrate RDL reports from their SSRS servers to Power BI. It is available on GitHub, and it documents an end-to-end walkthrough of the migration scenario. 

### High availability

The SQL Server high availability features Always On failover cluster instances and Always On availability groups become obsolete on the target Azure SQL Database as high availability architecture is already built into both [General Purpose (standard availability model)](../../database/high-availability-sla.md#basic-standard-and-general-purpose-service-tier-availability) and [Business Critical (premium availability model)](../../database/high-availability-sla.md#premium-and-business-critical-service-tier-availability) SQL Database. The Business Critical / Premium Service Tier also provides read scale-out that allows connecting into one of the secondary nodes for read-only purposes. 

Beyond the high availability architecture that is included in SQL Database, there is also the [auto-failover groups](../../database/auto-failover-group-overview.md) feature that allows you to manage the replication and failover of databases in a managed instance to another region. 

### SQL Agent jobs 
SQL Agent jobs are not directly supported in Azure SQL Database and will need to be deployed to [Elastic Database Jobs](/azure/azure-sql/database/job-automation-overview#elastic-database-jobs-preview).
> [!IMPORTANT]
> Elastic Database Jobs is currently in Preview

### Logins and groups 

SQL logins from the source SQL Server can be moved to Azure SQL Database using Database Migration Service (DMS) in offline mode.  Use the **Selected logins** blade in the **Migration Wizard** to migrate logins to your target SQL Database. 

Windows users and groups can also be migrated using DMS by enabling the corresponding toggle button in the DMS Configuration page. 

Alternatively, you can use the [PowerShell utility tool](https://github.com/microsoft/DataMigrationTeam/tree/master/IP%20and%20Scripts/MoveLogins) specially designed by the Microsoft Data Migration Architects. The utility uses PowerShell to create a T-SQL script to recreate logins and select database users from the source to the target. The tool automatically maps Windows AD accounts to Azure AD accounts, and can do a UPN lookup for each login against the source Active Directory. The tool scripts custom server and database roles, as well as role membership, database role, and user permissions. Contained databases are not yet supported and only a subset of possible SQL Server permissions are scripted. 


### System databases
For Azure SQL Database single databases and elastic pools, only [master Database](/sql/relational-databases/databases/master-database?view=sqlallproducts-allversions) and tempdb Database apply. For a discussion of tempdb in the context of Azure SQL Database, see [tempdb Database in Azure SQL Database](/sql/relational-databases/databases/tempdb-database?view=sqlallproducts-allversions#tempdb-database-in-sql-database).

## Leverage advanced features 

Be sure to take advantage of the advanced cloud-based features offered by SQL Database. For example, you no longer need to worry about managing backups as the service does it for you. You can restore to any [point in time within the retention period](../../database/recovery-using-backups.md#point-in-time-restore). 

To strengthen security, consider using [Azure Active Directory Authentication](../../database/authentication-aad-overview.md), [auditing](../../managed-instance/auditing-configure.md), [threat detection](../../database/advanced-data-security.md), [row-level security](/sql/relational-databases/security/row-level-security), and [dynamic data masking](/sql/relational-databases/security/dynamic-data-masking).

In addition to advanced management and security features, SQL Database provides a set of advanced tools that can help you [monitor and tune your workload](../../database/monitor-tune-overview.md). [Azure SQL Analytics](/azure/azure-monitor/insights/azure-sql) is an advanced cloud monitoring solution for monitoring performance of all of your Azure SQL databases at scale and across multiple subscriptions in a single view. Azure SQL Analytics collects and visualizes key performance metrics with built-in intelligence for performance troubleshooting.
> [!IMPORTANT]
> Azure SQL Analytics is currently in Preview

[Automatic tuning](/sql/relational-databases/automatic-tuning/automatic-tuning#automatic-plan-correction) in managed instances continuously monitors performance of your SQL plan execution statistics and automatically fixes the identified performance issues. 

## SQL Server on Azure VM alternative

Your business may have requirements that make [SQL Server on Azure Virtual Machines](/azure/azure-sql/virtual-machines/windows/sql-server-on-azure-vm-iaas-what-is-overview) a more suitable target than Azure SQL Database. 

If the following apply to your business, consider moving to a SQL Server on Azure VM instead: 

- If you require direct access to the operating system or file system, such as to install third-party or custom agents on the same virtual machine with SQL Server. 
- If you have strict dependency on features that are still not supported, such as FileStream/FileTable, PolyBase, and cross-instance transactions. 
- If you absolutely need to stay at a specific version of SQL Server (2012, for instance). 
- If your compute requirements are much lower than managed instance offers (one vCore, for instance), and database consolidation is not an acceptable option. 

## Migration assets 

For additional assistance, see the following resources which were developed for real world migration projects.

|Asset  |Description  |
|---------|---------|
|[Data workload assessment model and tool](https://github.com/Microsoft/DataMigrationTeam/tree/master/Data%20Workload%20Assessment%20Model%20and%20Tool)| This tool provides suggested "best fit" target platforms, cloud readiness, and application/database remediation level for a given workload. It offers simple, one-click calculation and report generation that helps to accelerate large estate assessments by providing and automated and uniform target platform decision process.|
|[DBLoader Utility](https://github.com/microsoft/DataMigrationTeam/tree/master/DBLoader%20Utility)|The DBLoader can be used to load data from delimited text files into SQL Server. This Windows console utility uses the SQL Server native client bulk load interface, which works on all versions of SQL Server, including Azure SQL DB.|
|[Convert SQL Server Agent Jobs into Elastic Database Jobs](https://github.com/Microsoft/DataMigrationTeam/tree/master/IBM%20DB2%20LUW%20Inventory%20Scripts%20and%20Artifacts)|This script migrates your source SQL Server Agent Jobs to Elastic Database Jobs.|
|[Utility to move On-Premises SQL Server Logins to Azure SQL DB](https://github.com/microsoft/DataMigrationTeam/tree/master/IP%20and%20Scripts/MoveLogins)|A PowerShell script that creates a T-SQL command script to re-create logins and select database users from on-premises SQL Server to Azure SQL Database. The tool allows automatic mapping of Windows AD accounts to Azure AD accounts as well as optionally migrating SQL Server native logins.|
|[Perfmon data collection automation using Logman](https://github.com/microsoft/DataMigrationTeam/tree/master/IP%20and%20Scripts/Perfmon%20Data%20Collection%20Automation%20Using%20Logman)|A tool that collects Perform data to understand baseline performance that assists in the migration target recommendation. This tool that uses logman.exe to create the command which will create, start, stop and delete performance counters set on a remote SQL Server|
|[Whitepaper - Database migration to Azure SQL DB using BACPAC](https://github.com/microsoft/DataMigrationTeam/blob/master/Whitepapers/Database%20migrations%20-%20Benchmarks%20and%20Steps%20to%20Import%20to%20Azure%20SQL%20DB%20Single%20Database%20from%20BACPAC.pdf)|This whitepaper provides guidance and steps to help accelerate migrations from SQL Server to Azure SQL Database using BACPAC files.|

These resources were developed as part of the Data SQL Ninja Program, which is sponsored by the Azure Data Group engineering team. The core charter of the Data SQL Ninja program is to unblock and accelerate complex modernization and compete data platform migration opportunities to Microsoft's Azure Data platform. If you think your organization would be interested in participating in the Data SQL Ninja program, please contact your account team and ask them to submit a nomination.


## Partners

The following partners can provide alternative methods for migration as well: 

:::row:::
   :::column span="":::
      [:::image type="content" source="media/sql-server-to-database-overview/Blitzz_logo_84.png" alt-text="Blitzz":::](https://www.blitzz.io/product)
   :::column-end:::
   :::column span="":::
      [:::image type="content" source="media/sql-server-to-database-overview/blueprint_logo.png" alt-text="Blueprint":::](https://bpcs.com/what-we-do)
   :::column-end:::
   :::column span="":::
      [:::image type="content" source="media/sql-server-to-database-overview/Cognizant-220.1.png" alt-text="Cognizant":::](https://www.cognizant.com/partners/microsoft)
   :::column-end:::   
:::row-end:::
:::row:::
   :::column span="":::
      [:::image type="content" source="media/sql-server-to-database-overview/commvault-220.png" alt-text="Commvault":::](https://www.commvault.com/supported-technologies/microsoft)
   :::column-end:::   
   :::column span="":::
      [:::image type="content" source="media/sql-server-to-database-overview/DataSunrise_database_security_logo.png" alt-text="DataSunrise":::](https://www.datasunrise.com/)
   :::column-end:::
   :::column span="":::
      [:::image type="content" source="media/sql-server-to-database-overview/dbbest-logo.png" alt-text="DBBTest":::](https://www.dbbest.com/)
   :::column-end:::   
:::row-end:::
:::row:::
   :::column span="":::
      [:::image type="content" source="media/sql-server-to-database-overview/DXC_logo_cropped.png" alt-text="DXC":::](https://www.dxc.technology/application_services/offerings/139843/142343-application_services_for_microsoft_azure)
   :::column-end:::
   :::column span="":::
     [:::image type="content" source="media/sql-server-to-database-overview/InfosysLogo.png" alt-text="Infosys":::](https://www.infosys.com/services/)
   :::column-end:::
   :::column span="":::
      [:::image type="content" source="media/sql-server-to-database-overview/nayatech_migVisor_logo_small.png" alt-text="MigVisor":::](https://www.migvisor.com/)
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="":::
      [:::image type="content" source="media/sql-server-to-database-overview/querysurge_logo-84.png" alt-text="Querysurge":::](https://www.querysurge.com/company/partners/microsoft)
   :::column-end:::
   :::column span="":::
     [:::image type="content" source="media/sql-server-to-database-overview/quest_logo_cropped1.png" alt-text="Quest":::](https://www.quest.com/products/shareplex/)
   :::column-end:::   
   :::column span="":::
     [:::image type="content" source="media/sql-server-to-database-overview/rhipe-logo-small_final1.png" alt-text="Rhipe":::](https://www.rhipe.com/services/azure-migration/)
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="":::
     [:::image type="content" source="media/sql-server-to-database-overview/scalability-experts-logo3.png" alt-text="Scalability Experts":::](http://www.scalabilityexperts.com/products/index.html)
   :::column-end:::   
   :::column span="":::
     [:::image type="content" source="media/sql-server-to-database-overview/wipro-220.png" alt-text="Wipro":::](https://www.wipro.com/analytics/)
   :::column-end:::
   :::column span="":::
     [:::image type="content" source="media/sql-server-to-database-overview/Zen3-logo-220.png" alt-text="Zen":::](https://www.zen3.com/cloud-migration/)
   :::column-end:::
:::row-end:::


## Next steps

To start migrating your SQL Server to Azure SQL Database, see the [SQL Server to SQL Database migration guide](sql-server-to-database-guide.md).

- For a matrix of the Microsoft and third-party services and tools that are available to assist you with various database and data migration scenarios as well as specialty tasks, see [Service and tools for data migration](../../../dms/dms-tools-matrix.md).

- To learn more about Azure SQL Database see:

   - [Azure total Cost of Ownership Calculator](https://azure.microsoft.com/pricing/tco/calculator/) 


- To learn more about the framework and adoption cycle for Cloud migrations, see
   -  [Cloud Adoption Framework for Azure](/azure/cloud-adoption-framework/migrate/azure-best-practices/contoso-migration-scale)
   -  [Best practices for costing and sizing workloads migrate to Azure](/azure/cloud-adoption-framework/migrate/azure-best-practices/migrate-best-practices-costs) 


- To assess the Application access layer, see [Data Access Migration Toolkit (Preview)](https://marketplace.visualstudio.com/items?itemName=ms-databasemigration.data-access-migration-toolkit)
- For details on how to perform Data Access Layer A/B testing see [Database Experimentation Assistant](/sql/dea/database-experimentation-assistant-overview).
