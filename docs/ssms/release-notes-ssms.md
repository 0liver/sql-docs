---
title: Release notes for (SSMS)
description: Release notes for SQL Server Management Studio (SSMS).
author: erinstellato-ms
ms.author: erinstellato
ms.reviewer: maghan
ms.date: 03/19/2024
ms.service: sql
ms.subservice: ssms
ms.topic: conceptual
---

# Release notes for SQL Server Management Studio (SSMS)

[!INCLUDE [sql-asdb-asdbmi-asa](../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

This article details updates, improvements, and bug fixes for the current and previous versions of SSMS.

[!INCLUDE[ssms-connect-aazure-ad](../includes/ssms-connect-azure-ad.md)]

## Current SSMS release

:::image type="icon" source="../includes/media/download.svg" border="false"::: **[Download SQL Server Management Studio (SSMS) 20.0](https://aka.ms/ssmsfullsetup)**

SSMS 20.0 is the latest general availability (GA) release of SSMS. If you need a previous version of SSMS, see [previous SSMS releases](release-notes-ssms.md#previous-ssms-releases).

### 20.0

- Release number: 20.0
- Build number: 20.0.70.0
- Release date: March 19, 2024

Available languages:

- [Chinese (Simplified)](https://go.microsoft.com/fwlink/?linkid=2264204&clcid=0x804)
- [Chinese (Traditional)](https://go.microsoft.com/fwlink/?linkid=2264204&clcid=0x404)
- [English (United States)](https://go.microsoft.com/fwlink/?linkid=2264204&clcid=0x409)
- [French](https://go.microsoft.com/fwlink/?linkid=2264204&clcid=0x40c)
- [German](https://go.microsoft.com/fwlink/?linkid=2264204&clcid=0x407)
- [Italian](https://go.microsoft.com/fwlink/?linkid=2264204&clcid=0x410)
- [Japanese](https://go.microsoft.com/fwlink/?linkid=2264204&clcid=0x411)
- [Korean](https://go.microsoft.com/fwlink/?linkid=2264204&clcid=0x412)
- [Portuguese (Brazil)](https://go.microsoft.com/fwlink/?linkid=2264204&clcid=0x416)
- [Russian](https://go.microsoft.com/fwlink/?linkid=2264204&clcid=0x419)
- [Spanish](https://go.microsoft.com/fwlink/?linkid=2264204&clcid=0x40a)

#### What's new in 20.0

| Feature | Details |
| --- | --- |
| Connection | The connection security properties **Encryption** and **Trust server certificate** now exist on the main sign in page in the Connection dialog for easier access. For more information, see [Connect with SQL Server Management Studio](quickstarts/ssms-connect.md). |
| Connection | A new property, **Host Name in Certificate**, used with the *Strict (SQL Server 2022 and Azure SQL)* and *Mandatory* **Encryption** options, now exists on the Login page of the Connection dialog. |
| Connection | Added icons to the Query Editor status bar to indicate the encryption method used for the connection. |
| Connection | Added Microsoft Entra ID authentication to **New Login** wizard. |
| Drivers | Updated Microsoft.Data.SqlClient version to 5.1.4, which includes support for Strict encryption and Transport Layer Security (TLS) 1.3. |
| Libraries | Updated Server Management Objects (SMO) version to 171.30.0|
| Libraries | Updated DacFx version to 162.1.x. |
| Options | Introduced a new option, **Trust server certificate for imported connections**, in **Tools > Options > SQL Server Object Explorer > Commands** under **Connection security**. For more information, see [Options (SQL Server Object Explorer - Commands)](menu-help/options-sql-server-object-explorer-commands.md). |

#### Bug fixes in 20.0

| Feature | Details |
| --- | --- |
| Always Encrypted | The **New Column Master Key** dialog supports Azure Key Vault using role permissions for authorization. |
| Connection | Resolved an issue with SSMS crashing when trying to connect to Azure Storage because the user didn't have access to any containers within the storage account. |
| Connection | Fixed an issue where users couldn't change their password with **Trust server certificate** enabled. |
| Link feature for Azure SQL Managed Instance | Improved handling for importing and deleting certificates. |
| Link feature for Azure SQL Managed Instance | Addressed issues related to text and images in the Link wizards. |
| Security | Addressed vulnerability [CVE-2023-2975](https://nvd.nist.gov/vuln/detail/CVE-2023-2975) for the OpenSSL library. |

#### Known issues 20.0

| Feature | Details | Workaround |
| --- | --- | --- |
| Analysis Services | When you connect to Analysis Services with Microsoft Entra MFA, if you add a new role or open properties for a role, the message "the identity of the user being added to the role isn't fetched properly" appears. | This error is benign and can be ignored. The error is addressed within the Azure infrastructure, and no updates to SSMS are required. |
| Analysis Services | After adding a new role, or when opening properties for an existing role, you can't use **Search by name or email address** to add a user. | A user can be added with the **Manual Entry** option. |
| Database Designer | Selecting the Design option for a view referencing a table using spatial data causes SSMS to crash. | Use T-SQL to make changes to the view. |
| Database Mirroring | If you launch the Database Mirroring Monitor from the mirrored node, the primary node isn't listed. | Register the mirrored node from Database Mirroring Monitoring, or use SSMS 18.12.1 to monitor from the mirrored node. |
| General SSMS | Import settings from SSMS 17 option not available. | Settings can be imported from SSMS 18. |
| Link feature for Azure SQL Managed Instance | After you remove an existing mirroring endpoint certificate on [!INCLUDE [ssnoversion-md](../includes/ssnoversion-md.md)], link creation through the wizard might fail due to unestablished trust between [!INCLUDE [ssnoversion-md](../includes/ssnoversion-md.md)] and Azure SQL Managed Instance, even though all checks are successful. | Use PowerShell command `Get-AzSqlInstanceServerTrustCertificate` to check whether [!INCLUDE [ssnoversion-md](../includes/ssnoversion-md.md)] mirroring endpoint certificate named `<SQL_Server_Instance_Name>` exists in the Azure SQL Managed Instance. If so, use PowerShell command `Remove-AzSqlInstanceServerTrustCertificate` to remove it before a new link creation attempt. |
| Linked servers | Creating a linked server to Azure SQL Database with [!INCLUDE [ssnoversion-md](../includes/ssnoversion-md.md)] selected as Server type connects to the `master` database. | To create a linked server to Azure SQL Database, select **Other data source** for the **Server type**, and select **Microsoft OLE DB Provider for SQL Server** or **Microsoft OLE DB Driver for SQL Server** as the **Provider**. Enter the logical server name in the Data source field and the database name in the Catalog field. |
| Maintenance Plans | Selecting the **Files and Filegroups** radio button in the Backup Database Task causes the dialog to close unexpectedly. | No current alternative. |
| PolyBase | PolyBase node isn't visible in Object Explorer when you connect to [!INCLUDE [sssql22-md](../includes/sssql22-md.md)]. | Use SSMS 18.12.1. |
| Profiler | The Profiler menu isn't localized. | No current alternative. |
| Replication | If Azure SQL Managed Instance is the publisher and SSMS is running on a machine that isn't in the same virtual network as the publisher, you aren't able to insert a tracer token via Replication Monitor. | To insert tracer tokens, use Replication Monitor in SSMS on a machine that is in the same virtual network as the Azure SQL Managed Instance publisher. |
| Stretch Database | Removed Stretch Database Wizard. | Use T-SQL to configure Stretch Database or use SSMS 18.9.1 or earlier to use the Stretch Database Wizard. |

#### Known issues using Strict encryption 20.0

| Feature | Details | Workaround |
| --- | --- | --- |
| Connection | When SQL Server is configured with **Force Strict Encryption**, selecting **Azure Data Studio > New Query** from the server or database menu generates the error, "A connection was successfully established with the server, but then an error occurred during the pre-login handshake. (provider: TCP provider, error: 0 - An existing connection was forcibly closed by the remote host.)" | Update the connection to use *Strict* instead of *Mandatory* for the **Encrypt** property in Azure Data Studio, and then connect. |
| Connection | Connecting to SQL Server with *Strict (SQL 2022 and Azure SQL)* selected for **Encryption** and a non-TCP/IP network protocol generates the error, "Cannot connect to SERVERNAME. A connection was successfully established with the server, but then an error occurred during the pre-login handshake. (provider: Shared Memory Provider, error: 15 - Function not supported) (Microsoft SQL Server, Error: 50)  The request is not supported" | Change the **Network protocol** connection property to use *TCP/IP*, or enable the TCP/IP protocol for the SQL Server. |
| Database Tuning Advisor | When SQL Server is configured with **Force Strict Encryption**, connection to the server from the Database Tuning Advisor isn't supported. | No alternative. |
| Maintenance Plans | When you connect to a server with *Strict (SQL Server 2022 and Azure SQL)* encryption, modifying an existing maintenance plan generates the error "Failed to connect to SERVERNAME. (Microsoft.SqlServer.ConnectionInfo) A connection was successfully established with the server, but then an error occurred during the login process. (provider: SSL Provider, error: 0 - The target principal name is incorrect.)" | The problem doesn't occur when you connect with *Mandatory* or *Optional* encryption. |
| Profiler | When SQL Server is configured with **Force Strict Encryption**, connection to the server from Profiler isn't supported, and the error "Cannot connect to SERVERNAME. Class not registered (pfutil)" is generated. | Install MSOLEDBSQL version 19, available from [Download Microsoft OLE DB Driver for SQL Server](../connect/oledb/download-oledb-driver-for-sql-server.md). |
| Profiler | When you connect to a server with *Strict (SQL Server 2022 and Azure SQL)* encryption and MSOLEDBSQL version 19 installed, traces can't be saved to, or loaded from, a database table. | No alternative. |
| PowerShell | When you connect to a server with *Strict (SQL Server 2022 and Azure SQL)* encryption, selecting **Start Powershell** from a node in Object Explorer generates the error "SQL Server PowerShell provider error: Could not connect to SERVERNAME. [Failed to connect to server SERVERNAME. --> A connection was successfully established with the server, but then an error occurred during the pre-login handshake. (provider: TCP Provider, error: 0 - An existing connection was forcibly closed by the remote host.) --> An existing connection was forcibly closed by the remote host]". | No current alternative. |
| SQL Server Logs | When SQL Server is configured with **Force Strict Encryption**, you can't view the SQL Server ERRORLOG files via Object Explorer, or executing `master.dbo.sp_enumerrorlogs` or `sys.xp_enumerrorlogs` via the Query Editor. | View the ERRORLOG files in the Log folder using File Explorer. |

You can reference [SQL user feedback](https://aka.ms/ssms-feedback) for other known issues (filter on **Tooling** under **Groups**) and to provide feedback to the product team.

## Previous SSMS releases

Download previous SSMS versions by selecting the download link in the related section.

| SSMS version | Build number | Release date |
| ------------ | ------------ | ------------ |
| [19.3](#193) | 19.3.4.0 | January 10, 2024 |
| [18.12.1](#18121) | 15.0.18420.0 | June 21, 2022 |
| [17.9.1](#1791) | 14.0.17289.0 | November 21, 2018 |
| [16.5.3](#1653) | 13.0.16106.4 | January 30, 2017 |

### 19.3

- Release number: 19.3
- Build number: 19.3.4.0
- Release date: January 10, 2024

[Chinese (Simplified)](https://go.microsoft.com/fwlink/?linkid=2257624&clcid=0x804) | [Chinese (Traditional)](https://go.microsoft.com/fwlink/?linkid=2257624&clcid=0x404) | [English (United States)](https://go.microsoft.com/fwlink/?linkid=2257624&clcid=0x409) | [French](https://go.microsoft.com/fwlink/?linkid=2257624&clcid=0x40c) | [German](https://go.microsoft.com/fwlink/?linkid=2257624&clcid=0x407) | [Italian](https://go.microsoft.com/fwlink/?linkid=2257624&clcid=0x410) | [Japanese](https://go.microsoft.com/fwlink/?linkid=2257624&clcid=0x411) | [Korean](https://go.microsoft.com/fwlink/?linkid=2257624&clcid=0x412) | [Portuguese (Brazil)](https://go.microsoft.com/fwlink/?linkid=2257624&clcid=0x416) | [Russian](https://go.microsoft.com/fwlink/?linkid=2257624&clcid=0x419) | [Spanish](https://go.microsoft.com/fwlink/?linkid=2257624&clcid=0x40a) |

#### What's new in 19.3

| Feature | Details |
| ---------- | ------- |
| Azure Data Studio installation integration | The installation of SSMS installs Azure Data Studio 1.47.1. |

#### Bug fixes in 19.3

| Feature | Details |
| -------- | ------- |
| Accessibility | Addressed issue with screen reader announcing incomplete or incorrect information in the database properties dialog. |
| Always Encrypted | Fixed inability to change the **Enable Secure Enclaves** option when creating a new Azure SQL Database using a non-English installation of SSMS. |
| Availability Groups | Changed text color for primary server name in the Availability Group Dashboard, which caused the entry to appear empty. |
| Extended Events | Changed text color for data column after selecting **View Target Data** for the ring_buffer target. |
| Installer | Fixed issue where users may be prompted to update SQL Server Management Studio even if the current release is installed, see [Bug in 19.2.56.2 update version detection](https://feedback.azure.com/d365community/idea/bb5e60b9-5d85-ee11-a81c-002248544521). |
| Object Explorer | Resolved crash occurring when trying to close Object Explorer while the tree is still expanding. |
| Reports | Updated Server Dashboard report to correctly show the number of schedulers when more than 255 are available, see [SSMS 19.2 - Reports - Server dashboard - Processors used by instance - wrong number](https://feedback.azure.com/d365community/idea/cdf3d393-a689-ee11-a81c-6045bdb7ea56).  |
| Security | Update to [Microsoft.Data.SqlClient 3.1.5](https://github.com/dotnet/SqlClient/blob/main/release-notes/3.1/3.1.5.md) to address [CVE-2024-0056](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2024-0056). |

#### Known issues 19.3

| Feature | Details | Workaround |
| -------- | ------- | ---------- |
| Analysis Services | When you connect to Analysis Services with Microsoft Entra MFA, if you add a new role or open properties for a role, the message “the identity of the user being added to the role is not fetched properly” appears. | This error is benign and can be ignored.  It will be addressed within the Azure infrastructure soon, and no updates to SSMS are required. |
| Analysis Services | After adding a new role, or when opening properties for an existing role, you can't use **Search by name or email address** to add a user. | A user can be added with the **Manual Entry** option. |
| Database Designer | Selecting the Design option for a view that references a table using spatial data causes SSMS to crash. | Use T-SQL to make changes to the view. |
| Database Mirroring | When you launch the Database Mirroring Monitor from the mirrored node, the primary node isn't listed. | Use SSMS 18.12.1 if you need to monitor mirroring from the mirrored node. |
| General SSMS | Import settings from SSMS 17 option not available. | Settings can be imported from SSMS 18. |
| Link feature for Azure SQL Managed Instance | After you remove an existing mirroring endpoint certificate on SQL Server, link creation through the wizard might fail due to unestablished trust between SQL Server and Azure SQL Managed Instance, even though all checks are successful. | Use PowerShell command ``Get-AzSqlInstanceServerTrustCertificate`` to check whether SQL Server mirroring endpoint certificate named "<SQL_Server_Instance_Name>" exists in the Azure SQL Managed Instance. If so, use PowerShell command ``Remove-AzSqlInstanceServerTrustCertificate`` to remove it before a new link creation attempt. |
| Linked servers | Creating a linked server to Azure SQL Database with SQL Server selected as Server type connects to the master database. | To create a linked server to Azure SQL Database, select **Other data source** for the **Server type**, and select **Microsoft OLE DB Provider for SQL Server** or **Microsoft OLE DB Driver for SQL Server** as the **Provider**. Enter logical server name in the Data source field, and enter database name in the Catalog field. |
| PolyBase | PolyBase node isn't visible in Object Explorer when connecting to SQL 2022. | Use SSMS 18.12.1. |
| Profiler | The Profiler menu isn't localized. | No current alternative. |
| Replication | If Azure SQL Managed Instance is the publisher and SSMS is running on a machine, which isn't in the same vNet as the publisher, you are not able to insert a tracer token via Replication Monitor. | To insert tracer tokens, use Replication Monitor in SSMS on a machine that is in the same vNet as the Azure SQL Managed Instance publisher. |
| Stretch DB | Removed Stretch DB Wizard. | Use T-SQL to configure Stretch DB or use SSMS 18.9.1 or earlier to use the Stretch DB Wizard. |

### 18.12.1

:::image type="icon" source="../includes/media/download.svg" border="false"::: [Download SSMS 18.12.1](https://go.microsoft.com/fwlink/?linkid=2199013&clcid=0x409)

- Release number: 18.12.1
- Build number: 15.0.18424.0
- Release date: June 21, 2022

[Chinese (Simplified)](https://go.microsoft.com/fwlink/?linkid=2199013&clcid=0x804) | [Chinese (Traditional)](https://go.microsoft.com/fwlink/?linkid=2199013&clcid=0x404) | [English (United States)](https://go.microsoft.com/fwlink/?linkid=2199013&clcid=0x409) | [French](https://go.microsoft.com/fwlink/?linkid=2199013&clcid=0x40c) | [German](https://go.microsoft.com/fwlink/?linkid=2199013&clcid=0x407) | [Italian](https://go.microsoft.com/fwlink/?linkid=2199013&clcid=0x410) | [Japanese](https://go.microsoft.com/fwlink/?linkid=2199013&clcid=0x411) | [Korean](https://go.microsoft.com/fwlink/?linkid=2199013&clcid=0x412) | [Portuguese (Brazil)](https://go.microsoft.com/fwlink/?linkid=2199013&clcid=0x416) | [Russian](https://go.microsoft.com/fwlink/?linkid=2199013&clcid=0x419) | [Spanish](https://go.microsoft.com/fwlink/?linkid=2199013&clcid=0x40a)

#### What's new in 18.12.1

| New Item | Details |
| -------- | ------- |
| Azure Data Studio installation integration | Installation of SSMS installs Azure Data Studio 1.37. |

#### Bug fixes in 18.12.1

| New Item | Details |
| -------- | ------- |
| Always Encrypted | Fixed issue with Column Master Key creation generating an exception when using Azure Key Vault as the key store. |
| Data Classification | Fixed issue with "Couldn't load file or assembly 'Microsoft.Information.Protection', Version=1.10.98.0" after upgrading to SSMS 18.10 or higher. See [Latest SSMS 18.11.1 breaks the Data Classification. Get missing assembly error after updating](https://feedback.azure.com/d365community/idea/af89d5d7-45a4-ec11-a81c-0022484ee92d). |
| SSMS General | Resolved error related to dacpac deployment using the Deploy Data-tier application option in Azure SQL DB with MFA. |

### 17.9.1

:::image type="icon" source="../includes/media/download.svg" border="false"::: [Download SSMS 17.9.1](https://go.microsoft.com/fwlink/?linkid=2043154&clcid=0x409)

- Release number: 17.9.1
- Build number: 14.0.17289.0
- Release date: November 21, 2018

[Chinese (Simplified)](https://go.microsoft.com/fwlink/?linkid=2043154&clcid=0x804)| [Chinese (Traditional)](https://go.microsoft.com/fwlink/?linkid=2043154&clcid=0x404)| [English (United States)](https://go.microsoft.com/fwlink/?linkid=2043154&clcid=0x409)| [French](https://go.microsoft.com/fwlink/?linkid=2043154&clcid=0x40c)| [German](https://go.microsoft.com/fwlink/?linkid=2043154&clcid=0x407)| [Italian](https://go.microsoft.com/fwlink/?linkid=2043154&clcid=0x410)| [Japanese](https://go.microsoft.com/fwlink/?linkid=2043154&clcid=0x411)| [Korean](https://go.microsoft.com/fwlink/?linkid=2043154&clcid=0x412)| [Portuguese (Brazil)](https://go.microsoft.com/fwlink/?linkid=2043154&clcid=0x416)| [Russian](https://go.microsoft.com/fwlink/?linkid=2043154&clcid=0x419)| [Spanish](https://go.microsoft.com/fwlink/?linkid=2043154&clcid=0x40a)

#### What's new in 17.9.1

SQL Server utility is no longer available in versions 17.x and newer.

#### Bug fixes in 17.9.1

- Fixed an issue where users could experience their connection being closed and reopened with each query invocation when using "Azure Active Directory - Universal with MFA support" authentication with the SQL query editor. Side effects of the connection closing included global temporary tables being dropped unexpectedly and sometimes a new session ID (SPID) given to the connection.
- Fixed a long outstanding issue where a restore plan would fail to find a restore plan or generate an inefficient one under certain conditions.
- Fixed issue in the "Import Data-tier Application" wizard, which could result in an error when connected to an Azure SQL Database.

> [!NOTE]  
> Non-English localized releases of SSMS 17.x require the [KB 2862966 security update package](https://support.microsoft.com/kb/2862966) if installed on: Windows 8, Windows 7, Windows Server 2012, and Windows Server 2008 R2.

#### Uninstall and reinstall SSMS 17.x

If your SSMS installation is having problems, and a standard uninstall and reinstall doesn't resolve them, you can first try [repairing](https://support.microsoft.com/help/4028054/windows-10-repair-or-remove-programs) the Visual Studio 2015 IsoShell. If repairing the Visual Studio 2015 IsoShell doesn't resolve the problem, the following steps have been found to fix many random issues:

1. Uninstall SSMS the same way you uninstall any application (using **Add or remove programs**).

1. Uninstall Visual Studio 2015 IsoShell **from an elevated cmd prompt**:

    ```PUSHD "C:\ProgramData\Package Cache\FE948F0DAB52EB8CB5A740A77D8934B9E1A8E301\redist"```
    ```vs_isoshell.exe /Uninstall /Force /PromptRestart```

1. Uninstall Microsoft Visual C++ 2015 Redistributable the same way you uninstall any application. Uninstall both x86 and x64 if they're on your computer.

1. Reinstall Visual Studio 2015 IsoShell **from an elevated cmd prompt**:

    ```PUSHD "C:\ProgramData\Package Cache\FE948F0DAB52EB8CB5A740A77D8934B9E1A8E301\redist"```
    ```vs_isoshell.exe /PromptRestart```

1. Reinstall SSMS.

1. Upgrade to the [latest version of the Visual C++ 2015 Redistributable](https://support.microsoft.com/help/2977003/the-latest-supported-visual-c-downloads) if you're not currently up to date.

### 16.5.3

:::image type="icon" source="../includes/media/download.svg" border="false"::: [Download SSMS 16.5.3](https://go.microsoft.com/fwlink/?LinkID=840946)

- Release number: 16.5.3
- Build number: 13.0.16106.4
- Release date: January 30, 2017

[Chinese (Simplified)](https://go.microsoft.com/fwlink/?linkid=840946&clcid=0x804)| [Chinese (Traditional)](https://go.microsoft.com/fwlink/?linkid=840946&clcid=0x404)| [English (United States)](https://go.microsoft.com/fwlink/?linkid=840946&clcid=0x409)| [French](https://go.microsoft.com/fwlink/?linkid=840946&clcid=0x40c)| [German](https://go.microsoft.com/fwlink/?linkid=840946&clcid=0x407)| [Italian](https://go.microsoft.com/fwlink/?linkid=840946&clcid=0x410)| [Japanese](https://go.microsoft.com/fwlink/?linkid=840946&clcid=0x411)| [Korean](https://go.microsoft.com/fwlink/?linkid=840946&clcid=0x412)| [Portuguese (Brazil)](https://go.microsoft.com/fwlink/?linkid=840946&clcid=0x416)| [Russian](https://go.microsoft.com/fwlink/?linkid=840946&clcid=0x419)| [Spanish](https://go.microsoft.com/fwlink/?linkid=840946&clcid=0x40a)

#### Bug fixes in 16.5.3

- Fixed issue introduced in SSMS 16.5.2, which was causing the expansion of the 'Table' node when the table had more than one sparse column.

- Users can deploy SQL Server Integration Services (SSIS) packages containing OData Connection Manager, which connects to a Microsoft Dynamics AX/CRM Online resource to the SSIS catalog. For more information, For details, see [OData Connection Manager](../integration-services/connection-manager/odata-connection-manager.md).

- Configuring Always Encrypted on an existing table fails with errors on unrelated objects. [Connect ID 3103181](https://connect.microsoft.com/SQLServer/feedback/details/3103181/setting-up-always-encrypted-on-an-existing-table-fails-with-errors-on-unrelated-objects)

- Configuring Always Encrypted for an existing database with multiple schemas doesn't work. [Connect ID 3109591](https://connect.microsoft.com/SQLServer/feedback/details/3109591/sql-server-2016-always-encrypted-against-existing-database-with-multiple-schemas-doesnt-work)

- The Always Encrypted, Encrypted Column wizard fails due to the database containing views that reference system views. [Connect ID 3111925](https://connect.microsoft.com/SQLServer/feedback/details/3111925/sql-server-2016-always-encrypted-encrypted-column-wizard-failed-task-failed-due-to-following-error-cannot-save-package-to-file-the-model-has-build-blocking-errors)

- When you encrypt with Always Encrypted, errors from refreshing modules after encryption are incorrectly handled.

- *Open recent* menu doesn't show recently saved files. [Connect ID 3113288](https://connect.microsoft.com/SQLServer/feedback/details/3113288/ssms-2016-open-recent-menu-doesnt-show-recently-saved-files)

- SSMS is slow when right-clicking an index for a table (over a remote (Internet) connection). [Connect ID 3114074](https://connect.microsoft.com/SQLServer/feedback/details/3114074/ssms-slow-when-right-clicking-an-index-for-a-table-over-a-remote-internet-connection)

- Fixed an issue with the SQL Designer scrollbar. [Connect ID 3114856](https://connect.microsoft.com/SQLServer/feedback/details/3114856/bug-in-scrollbar-on-sql-desginer-in-ssms-2016)

- Context menu for tables momentarily stops responding.

- SSMS occasionally throws exceptions in Activity Monitor and crashes. [Connect ID 697527](https://connect.microsoft.com/SQLServer/feedback/details/697527/).

- SSMS 2016 crashes with the error "The process was terminated due to an internal error in the .NET Runtime at IP 71AF8579 (71AE0000) with exit code 80131506".

## Additional Downloads

For a list of all SQL Server Management Studio downloads, search the [Microsoft Download Center](https://www.microsoft.com/download/search.aspx?q=sql%20server%20management%20studio&p=0&r=10&t=&s=Relevancy~Descending).

For the latest release of SQL Server Management Studio, see [Download SQL Server Management Studio (SSMS)](download-sql-server-management-studio-ssms.md).

## Next steps

- [Download SSMS](download-sql-server-management-studio-ssms.md)
- [Download Azure Data Studio](../azure-data-studio/download-azure-data-studio.md)
- [Azure Data Studio release notes](../azure-data-studio/release-notes-azure-data-studio.md)
