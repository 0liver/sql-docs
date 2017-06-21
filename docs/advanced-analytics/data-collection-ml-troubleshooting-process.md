---
title: "Data Collection for Machine Learning Troubleshooting"
ms.custom: ""
ms.date: "06/16/2017"
ms.prod: "sql-server-2016"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "r-services"
ms.tgt_pltfrm: ""
ms.topic: "article"
dev_langs: 
  - "R"
caps.latest.revision: 1
author: "jeannt"
ms.author: "jeannt"
manager: "jhubbard"
---
# Data Collection for Machine Learning Troubleshooting

This article describes the kind of data that you should collect when attempting to resolve problems with the setup, configuration, or performance of machine learning in SQL Server. Such data includes logs, error messages, and system information.

The goal is to describe the sources of information that will be most useful to you when performing diagnostics on a self-help basis. Collection of this information will also be useful when you request technical support for an issue related to SQL Server machine learning features.

**Applies to:** SQL Server 2016 R Services, SQL Server 2017 Machine Learning Services (R and Python)

## SQL Server and R Version

Note whether the installation was a new install or an upgrade. For upgrades, determine how the upgrade was performed:

- Which version did you upgrade from? 
- Did you remove old components, or upgrade in place?
- Did you change any feature selections when upgrading? 

### Which version of SQL Server is installed? Which edition? 

SQL Server R Services was introduced in SQL Server 2016. Previous versions do not support machine learning. Moreover, subsequent service packs for the 2016 release included many bug fixes and improvements. AS a first step, you should consider installing Service Pack 1 or later.

In SQL Server 2017, support was extended to the Python language. Support for Python was not provided in earlier releases.

If you need help determining which version and edition you have, see this article, which lists the build numbers for each of the
[SQL Server Versions](https://social.technet.microsoft.com/wiki/contents/articles/783.sql-server-versions.aspx#Service_Pack_editions)

Depending on the **edition** of SQL Server that you are using, some machine learning functionality might be unavailable, or limited.

See the following topics for a list of machine learning features in Enterprise, Developer, Standard, and Express editions.

+ [Editions and Supported Features of SQL Server](https://docs.microsoft.com/sql/sql-server/editions-and-components-of-sql-server-2016)
+ [Differences in R Features between Editions of SQL Server](https://docs.microsoft.com/sql/advanced-analytics/r/differences-in-r-features-between-editions-of-sql-server)

### Which version of Microsoft R is installed?

In general, the version of Microsoft R that is installed when you select the R Services feature, or the Machine Learning Services feature, is determined by the SQL Server build number. If you upgrade or patch the SQL Server, you must also upgrade or patch the R components.

For the list of releases and links to R component downloads, see Installing ML Components without Internet Access. On computers with Internet access, the required version of R is identified and installed automatically.

However, it is possible to upgrade the R Server components separately from the SQL Server database engine, in a process known as binding. 

Therefore, the version of R that you use when running R code in SQL Server may differ depending both on the installed version of SQL Server, and whether you have migrated the server to the latest R version.

#### To determine the R version

The easiest way to determine the R version is to run a statement such as the following to get the runtime properties.

```SQL
exec sp_execute_external_script
       @language = N'R'
       , @script = N'
# Transform R version properties to data.frame
OutputDataSet <- data.frame(
  property_name = c("R.version", "Revo.version"), 
  property_value = c(R.Version()$version.string, Revo.version$version.string),
  stringsAsFactors = FALSE)
# Retrieve properties like R.home, libPath & default packages
OutputDataSet <- rbind(OutputDataSet, data.frame(
  property_name = c("R.home", "libPaths", "defaultPackages"),
  property_value = c(R.home(), .libPaths(), paste(getOption("defaultPackages"), collapse=", ")),
  stringsAsFactors = FALSE)
)
'
WITH RESULT SETS ((PropertyName nvarchar(100), PropertyValue nvarchar(4000)));

```

> [!TIP] 
> If R Services is not working, try running just the R script portion from RGui.

As a last resort, you can open files on the server to determine the version that was installed. To do this, locate the file, rlauncher.config, to get the location of the R runtime and the current working directory. We recommend that you make a copy and open the copy so that you don't accidentally change anything.

+ SQL Server 2016
  
  `C:\Program Files\Microsoft SQL Server\MSSQL13.<instance_name\MSSQL\Binn\rlauncher.config`

+ SQL Server 2017
  
  `C:\Program Files\Microsoft SQL Server\MSSQL14.<instance_name>\MSSQL\Binn\rlauncher.config`

To get the R version and RevoScaleR versions, open an R command prompt, or open the RGui associated with the instance.

+ SQL Server 2016
  
  `C:\Program Files\Microsoft SQL Server\MSSQL13.<instancename>\R_SERVICES\bin\x64\RGui.exe`

+ SQL Server 2017
  
  `C:\Program Files\Microsoft SQL Server\MSSQL14.<instance_name>\R_SERVICES\bin\x64\RGui.exe`


The R console displays the version information on startup. For example, the following version represents the default configuration for SQL Server 2017 CTP 2.0.

*Microsoft R Open 3.3.3*

*The enhanced R distribution from Microsoft*

*Microsoft packages Copyright (C) 2017 Microsoft*

*Loading Microsoft R Server packages, version 9.1.0.*


### What version of Python is installed?

Support for Python is available only in SQL Server 2017 CTP 2.0 and later.

There are several ways to get the Python version. The easiest way is to run this statement from Management Studio or any other SQL query tool:

```SQL
-- Get Python runtime properties:
exec sp_execute_external_script
       @language = N'Python'
       , @script = N'
import sys
import pkg_resources
OutputDataSet = pandas.DataFrame(
                    {"property_name": ["Python.home", "Python.version", "Revo.version", "libpaths"],
                    "property_value": [sys.executable[:-10], sys.version, pkg_resources.get_distribution("revoscalepy").version, str(sys.path)]}
)
'
with result sets ((PropertyName nvarchar(100), PropertyValue nvarchar(4000)));
```

If for some reason Machine Learning Services is not running, you can determine the Python version that was installed by looking at the file, pythonlauncher.config. We recommend that you make a copy of the file and open the copy, to avoid accidentally changing any properties.

1. For SQL Server 2017 only: `C:\Program Files\Microsoft SQL Server\MSSQL14.<instance_name>\MSSQL\Log\ExtensibilityLog\pythonlauncher.config `
2. Get the value for **PYTHONHOME**.
3. Get the value of the current working directory.


> [!NOTE]
> If you have installed both Python and R in SQL Server 2017, the working directory and the pool of worker accounts are shared for the R and Python languages.

### Are multiple instances of R or Python installed?

Check whether there is more than one copy of the R libraries installed on the computer. This can happen when:

+ During setup you select both R Services (In-Database) and R Server (Standalone 
+ You installed Microsoft R Client in addition to SQL Server
+ A different set of R libraries was installed, using R Tools for Visual Studio, R Studio, Microsoft R Client, or another R IDE.
+ The computer hosts multiple instances of SQL Server, and more than one instance uses machine learning.

The same applies to Python.

If you find that multiple libraries or runtimes are installed, make sure that you get only the errors associated with the Python or R runtimes used by the SQL Server instance.

## Errors and Messages

The errors that you see when you attempt to run  R code can come from any of the following sources:

- SQL Server database engine, including the stored procedure sp_execute_external_script
- The SQL Server Trusted Launchpad 
- Other components of the extensibility framework, including R and Python launchers and satellite processes
- Providers, such as ODBC
- R language

When working with the service for the first time, it can be difficult to tell which messages originate form which services. We recommend that you capture not only the exact message text, but the context in which you saw the message:

What client software are you using to run machine learning code:

- Management Studio? An external application?
- Are you running R code in a remote client, or directly in a stored procedure?

### What errors has SQL Server logged?

Get the most recent SQL Server ERRORLOG. The complete set of error logs is comprised of the files from the following default log directory:

+ SQL Server 2016
  
  `C:\Program Files\Microsoft SQL Server\MSSQL13.SQL2016\MSSQL\Log\ExtensibilityLog`

+ SQL Server 2017
  
  `C:\Program Files\Microsoft SQL Server\MSSQL14.SQL2016\MSSQL\Log\ExtensibilityLog`

> [!NOTE] 
> The exact folder name differs based on the instance name.


### What errors were returned by sp_execute_external_script?

Get the complete text of error that is returned, if any, when you run the sp_execute_external_script command. 

To remove R or Python problems from consideration, you can run this script, which starts the R or Python runtime and passes data back and forth.

**For R**

```sql
exec sp_execute_external_script @language =N'R',  
@script=N'OutputDataSet<-InputDataSet',  
@input_data_1 =N'select 1 as hello'  
with result sets (([hello] int not null));  
go
```

**For Python**

```sql
exec sp_execute_external_script @language =N'Python',  
@script=N'OutputDataSet= InputDataSet',  
@input_data_1 =N'select 1 as hello'  
with result sets (([hello] int not null));  
go
```

### What errors were generated by the extensibility framework?

SQL Server generates separate logs for the external script language runtimes. These are not the errors generated by the Python or R language, but rather errors from the extensibility components in SQL Server, including language-specific launchers and their satellite processes.

You can get these logs from the following default locations:

+ SQL Server 2016
  
  `C:\Program Files\Microsoft SQL Server\MSSQL13.<instance_name>\MSSQL\Log\ExtensibilityLog`

+ SQL Server 2017
  
  `C:\Program Files\Microsoft SQL Server\MSSQL14.<instance_name>\MSSQL\Log\ExtensibilityLog `

> [!NOTE] 
> The exact folder name will differ based on the instance name. Depending on your configuration, the folder could be on a different drive.

For example, the following log messages are related to the extensibility framework:

+ "LogonUser Failed for user MSSQLSERVER01"
  
  This might indicate that the worker accounts that run external scripts cannot access the instance.

+ "InitializePhysicalUsersPool Failed" 
  
  This might mean that your security settings are such that setup could not create the pool of worker accounts needed to run external scripts.

+ "Security Context Manager initialization failed" 

+ "Satellite Session Manager initialization failed"

### Are there any related system events?

1. Open Windows Event Viewer, and search the **System Event** log for messages including the text "Launchpad". 
2. Open the ExtLaunchErrorlog file and look for the word "ErrorCode". Review the message associated with the ErrorCode. 

For example, the following messages are some common system errors related to the SQL Server extensibility framework: 

+ "The SQL Server Launchpad (MSSQLSERVER) service failed to start due to the following error:  <text>"

+ "The service did not respond to the start or control request in a timely fashion." 

+ A timeout was reached (120000 milliseconds) while waiting for the SQL Server Launchpad (MSSQLSERVER) service to connect. 

### Did any components start and then crash?

If you are knowledgeable in debugging, you can use the dump files to analyze a failure in the Launchpad.

1. Locate the folder that contains the setup bootstrap logs for SQL Server. For example, in SQL Server 2016, the default path was: `C:\Program Files\Microsoft SQL Server\130\Setup Bootstrap\Log`
2. Open the subfolder of the bootstrap logs that is specific to extensibility.
3. If you need to submit a support request, you would add the entire contents of this folder to a zipped file. For example: 
  
  `C:\Program Files\Microsoft SQL Server\130\Setup Bootstrap\Log\LOG\ExtensibilityLog`
  
  The exact location might differ on your system, and could be on a different drive letter than C. Be sure to get the logs for the instance where machine learning is installed. 


## Related Tools and Configuration

This section lists additional components or providers that can be a source of errors when running R or Python scripts.

### What network protocols are available?

Machine Learning Services requires the following network protocols for internal communication among extensibility components, and for communication with external R or Python clients.

+ Named Pipes
+ TCP/IP

Open the SQL Server Configuration Manager to determine whether a protocol is installed, and if installed, whether it is enabled.

### Security configuration and permissions

For worker accounts

1. In Control Panel, open **Users and Groups**, and locate the group used to run external script jobs: by default, `SQLRUserGroup`.
2. Verify that the group exists and that it contains at least one worker account.
3. In SQL Server Management Studio, click the instance where R or Python jobs will be run, click Security, and determine whether there is a login for `SQLRUserGroup`.
4. Review permissions for the user group

For individual user accounts

1. Determine whether the instance supports Mixed Mode authentication, SQL logins only, or Windows authentication only. This setting affects your R or Python code requirements.
2. For each user who needs to run R code, determine the required level of permissions on each database where objects will be written from R, data accessed, or objects created.
3. Create roles or add users to the following roles as necessary to enable script execution:

  - All but `db_owner` require EXECUTE ANY EXTERNAL SCRIPT
  - To write results from R or Python, `db_datawriter`
  - To create new objects, `db_ddladmin`
  - To read data used by R or Python code, `db_datareader`
4. Did you change any default startup accounts when you installed SQL 2016?
5. If the user needs to install new R packages, or use R packages installed by other users, you might need to enable package management on the instance, and assign additional permissions. For more information, see LINK.

### What folders are subject to locking by antivirus software?

Antivirus software can lock folders, preventing both setup of the machine learning features, and successful script execution. Determine if any folders in the SQL Server tree are subject to virus scanning.

However, when multiple services or features are installed on an instance, it can be difficult to enumerate all possible folders used by the instance. For example, when new features are added, new folders must be identified and excluded.

Moreover, some features create new folders dynamically at runtime. For example, in-memory OLTP tables, stored procedures, and functions all create new directories at runtime. Often these folder names contain GUIDS and cannot be predicted. The SQL Server Trusted Launchpad creates new working directories for R and Python script jobs.

Because it might not be possible to exactly exclude all folders needed by the SQl Server process ad its features, we recommend that you exclude the entire SQL Server instance directory tree.

### Is the firewall open for SQL Server? Does the instance support remote connections?

1. Determine whether SQL Server supports remote connections: [Configure Remote Server Connections](../database-engine/configure-windows/view-or-configure-remote-server-connection-options-sql-server.md)

2. Determine whether a firewall rule has been created for SQL Server. For security reasons, in a default installation, it might not be possible for remote R or Python client to connect to the instance.

For more information, see [Troubleshooting Connecting to SQL Server](../database-engine/configure-windows/troubleshoot-connecting-to-the-sql-server-database-engine.md)

### Can you run R script outside of T-SQL?

You can try running the R runtime associated with the SQ Server instance by using other R tools. That way you can determine if the required libraries are installed.

A base installation of R includes multiple tools that can be used to run an R script from the command line, as well as RGui for interactive execution of scripts.

If the R runtime is functioning but your script returns errors, we recommend that you try debugging the script in a dedicated R development environment, such as  R Tools for Visual Studio.

We also recommend that you review and slightly rewrite the script to correct data types issues that might arise when moving data between R and the database engine.

[R Libraries and Data Types](r/r-libraries-and-data-types.md)

Additionally, you can use the **sqlrutils** package to bundle your R script in a format that is more easily consumed as a stored procedure.

+ [Generating a Stored Procedure for R Code using the sqlrutils Package](r/generating-an-r-stored-procedure-for-r-code-using-the-sqlrutils-package.md)

+ [How to Create a Stored Procedure using sqlrutils](r/how-to-create-a-stored-procedure-using-sqlrutils.md)

## See Also

[Troubleshooting Machine Learning in SQL Server](machine-learning-troubleshooting-faq.md)