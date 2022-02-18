---
title: "Major version differences"
description: A description of the differences between the OLE DB Driver 19 for SQL Server and the OLE DB Driver for SQL Server
ms.custom: ""
ms.date: "02/16/2022"
ms.prod: sql
ms.prod_service: "database-engine, sql-database, synapse-analytics, pdw"
ms.reviewer: ""
ms.technology: connectivity
ms.topic: "reference"
helpviewer_keywords: 
  - "MSOLEDBSQL, additional resources"
  - "MSOLEDBSQL19, additional resources"
  - "OLE DB Driver for SQL Server, additional resources"
  - "OLE DB Driver 19 for SQL Server, additional resources"
author: David-Engel
ms.author: v-davidengel
---
# Major version differences

## Encryption property changes

In the Microsoft OLE DB Driver 19 for SQL Server, there are a number changes made to the encrypt property/connection string keyword. First, the driver property `SSPROP_INIT_ENCRYPT` has been changed from a `VT_BOOL` to a `VT_BSTR`. The valid values of this property are `no`/`yes`/`true`/`false`/`Optional`/`Mandatory`/`Strict`. The valid values for the provider connection string keyword `Encrypt` have changed from `no`/`yes` to `no`/`yes`/`true`/`false`/`Optional`/`Mandatory`/`Strict`. Similarly for the `IDataInitialize` connection string keyword `Use Encryption for Data`, the valid values have changed from `true`/`false` to `no`/`yes`/`true`/`false`/`Optional`/`Mandatory`/`Strict`. The `Optional` value is synonymous to the old `no`/`false` values and the `Mandatory` value is synonymous to the old `yes`/`true` values. `Strict` is a new value added in version 19.0.0 of the OLE DB Driver for SQL Server and encrypts `PRELOGIN` packets in addition to all other communication with the server. `Strict` encryption is only supported on SQL Server endpoints that support TDS 8.0, otherwise the driver will fail to connect. The OLE DB Driver 19 for SQL Server will continue to support all legacy keywords for backwards compatibility.

Second, the default value has changed from `no`/`false` to `Mandatory`. This change means that connections are encrypted by default. Previously, the driver would encrypt connections if explicitly set by the user and/or mandated by the SQL Server when the server side property `Force Encryption` was set to `yes`. To use old default behavior, include `Encrypt=Optional;` in the provider connection string, or `Use Encryption for Data=Optional;` in the `IDataInitialize` connection string.

## Driver name changes

The new Microsoft OLE DB Driver 19 for SQL Server supports side by side installation with the older Microsoft OLE DB Driver for SQL Server. To be able to differentiate the drivers, the name was changed to include the major version number. To use the new driver in an application, the user must specify the new driver name. The new driver name, along with the corresponding CLSID, are specified in the updated `msoledbsql.h` header that must be included in the project. Connections through the `IDBInitialize` interface require no further changes since `CLSID_MSOLEDBSQL` will specify the CLSID of the OLE DB Driver 19 for SQL Server. Connections through the `IDataInitialize` interface must replace the value of the `Provider` keyword with `MSOLEDBSQL19` to use the Microsoft OLE DB Driver 19 for SQL Server. In graphical user interfaces such as data link properties or linked server setup in SSMS, "Microsoft OLE DB Driver 19 for SQL Server" must be selected from the list of installed providers.

## See also
[OLE DB Driver for SQL Server](../oledb/oledb-driver-for-sql-server.md)
[Using Connection String Keywords with OLE DB Driver](../oledb/applications/using-connection-string-keywords-with-oledb-driver-for-sql-server.md)
[Using Encryption Without Validation](../oledb/features/using-encryption-without-validation.md)
[Universal Data Link (UDL) Configuration](../oledb/help-topics/data-link-pages.md)
[SQL Server Login Dialog Box (OLE DB)](../oledb/help-topics/sql-server-login-dialog.md)
[Initialization and authorization properties (OLE DB driver)](../oledb/ole-db-data-source-objects/initialization-and-authorization-properties.md)
  