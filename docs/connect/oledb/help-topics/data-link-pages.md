---
title: "Universal Data Link (UDL) Configuration | Microsoft Docs"
description: "Universal Data Link (UDL) Configuration using Microsoft OLE DB Driver for SQL Server"
ms.custom: ""
ms.date: "01/21/2019"
ms.prod: sql
ms.prod_service: "database-engine, sql-database, sql-data-warehouse, pdw"
ms.reviewer: ""
ms.technology: connectivity
ms.topic: reference
ms.author: "v-beaziz"
author: bazizi
---
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]

[!INCLUDE[Driver_OLEDB_Download](../../includes/driver_oledb_download.md)]

# Universal Data Link (UDL) Configuration
## Connection Tab
Use the Connection tab to specify how to connect to your data using the Microsoft OLE DB Driver for SQL Server.

![Screenshot of OLE DB Data Link Pages - Connection Tab](./media/data-link-pages-connection-tab.png)

The Connection tab is provider-specific and displays only the connection properties that are required by the Microsoft OLE DB Driver for SQL Server.

|Option|Description|
|---   |---        |
|Select or enter a server name|Select a server name from the drop-down list, or type the location of the server where the database you want to access is located. Selecting the database on the server is a separate action. Refresh the list by clicking Refresh.
|Enter information to sign in to the server|You can select the following authentication options from this drop-down list: <ul><li>`Windows Authentication:` Authentication to SQL Server using the currently logged-in user's account</li><li>`SQL Server Authentication:` Authentication to SQL Server using login ID and password</li><li>`Active Directory - Integrated:` Integrated authentication using the currently logged-in user's account</li><li>`Active Directory - Password:` Active Directory authentication using login ID and password</li></ul>|
|Server SPN|If you use a trusted connection, you can specify a service principal name (SPN) for the server.|
|User name|Type the User ID to use for authentication when you sign in to the data source.|
|Password|Type the password to use for authentication when you sign in to the data source.|
|Blank password|Enables the specified provider to use a blank password in the connection string.|
|Allow saving password|Allows the password to be saved with the connection string. Whether the password is included in the connection string depends on the functionality of the calling application. <br/><br/>**NOTE:** If saved, the password is returned and saved unmasked and unencrypted.|
|Use strong encryption for data|When checked, data that is passed through the connection will be encrypted.|
|Trust server certificate|When checked, the server's certificate will be validated. Server's certificate must have the correct hostname of the server and issued by a trusted certificate authority.|
|Select the database|Type the database name that you want to access.|
|Attach a database file as a database name|Attach an SQL database file as a database name. Type the database name to use for the attached SQL database file.<br/><br/>For SQL Server versions 6.5 and earlier, type the database file name to attach. Then type the name of the single-file database file. To browse for the file, click the button.|
|Change Password|Displays Change SQL Server Password dialog. |
|Test Connection|Click to attempt a connection to the specified data source. If the connection fails, ensure that the settings are correct. For example, spelling errors and case sensitivity can cause failed connections.|

## Advanced Tab
![Screenshot of OLE DB Data Link Pages - Advanced Tab](./media/data-link-pages-advanced-tab.png)

|Option|Description|
|---   |---        |
| Connect timeout | Specifies the amount of time (in seconds) that Microsoft OLE DB Driver for SQL Server waits for initialization to complete. If initialization times out, an error is returned and the connection is not created.|


> [!NOTE]  
>  For more general Data Link connection information, see the [Data Link API Overview](https://docs.microsoft.com/en-us/previous-versions/windows/desktop/ms718102(v=vs.85)).