---
title: Download and install Microsoft SQL Operations Studio (preview) | Microsoft Docs
description: 'Download and Install Microsoft SQL Operations Studio (preview) for Windows, macOS, or Linux'
ms.custom: "tools|sos"
ms.date: "03/28/2018"
ms.prod: "sql-non-specified"
ms.reviewer: "alayu; erickang; sstein"
ms.suite: "sql"
ms.prod_service: sql-tools
ms.component: sos
ms.tgt_pltfrm: ""
ms.topic: "article"
author: "stevestein"
ms.author: "sstein"
manager: craigg
ms.workload: "Inactive"
---
# Download and install SQL Operations Studio (preview)

[!INCLUDE[name-sos](../includes/name-sos.md)] runs on Windows, macOS, and Linux.

Download and install the latest release, the *March Public Preview*:

|Platform|Download|Release date| Version |
|:---|:---|:---|:---|
|Windows|[Installer](https://go.microsoft.com/fwlink/?linkid=870837)<br>[.zip](https://go.microsoft.com/fwlink/?linkid=870838)|March 28, 2018 |0.27.3|
|macOS|[.zip](https://go.microsoft.com/fwlink/?linkid=870839)|March 28, 2018 |0.27.3|
|Linux|[.deb](https://go.microsoft.com/fwlink/?linkid=870842)<br>[.rpm](https://go.microsoft.com/fwlink/?linkid=870841)<br>[.tar.gz](https://go.microsoft.com/fwlink/?linkid=870840)|March 28, 2018 |0.27.3|

For details about the latest release, see the [release notes](release-notes.md).

## Get SQL Operations Studio (preview) for Windows

This release of [!INCLUDE[name-sos](../includes/name-sos-short.md)] includes a standard Windows installer experience, and a .zip: 

**Installer**

1. Download and run the [[!INCLUDE[name-sos](../includes/name-sos-short.md)] installer for Windows](https://go.microsoft.com/fwlink/?linkid=870837).
1. Start the [!INCLUDE[name-sos-short](../includes/name-sos-short.md)] app.


**.zip file**

1. Download [[!INCLUDE[name-sos](../includes/name-sos-short.md)] .zip for Windows](https://go.microsoft.com/fwlink/?linkid=870838).
2. Browse to the downloaded file and extract it.
3. Run `\sqlops-windows\sqlops.exe`


## Get SQL Operations Studio (preview) for macOS

1. Download [[!INCLUDE[name-sos](../includes/name-sos-short.md)] for macOS](https://go.microsoft.com/fwlink/?linkid=870839).
2. To expand the contents of the zip, double-click it.
3. To make [!INCLUDE[name-sos](../includes/name-sos-short.md)] available in the *Launchpad*, drag *sqlops.app* to the *Applications* folder.


## Get SQL Operations Studio (preview) for Linux

1. Download [[!INCLUDE[name-sos](../includes/name-sos-short.md)] for Linux](https://go.microsoft.com/fwlink/?linkid=870840).
1. To extract the file and launch [!INCLUDE[name-sos](../includes/name-sos-short.md)], open a new Terminal window and type the following commands:

   **Debian Installation:**
   ```bash
   cd ~
   sudo dpkg -i ./Downloads/sqlops-linux-<version string>.deb

   sqlops
   ```

   **rpm Installation:**
   ```bash
   cd ~
   yum install ./Downloads/sqlops-linux-<version string>.rpm

   sqlops
   ```

   > [!NOTE]
   > On Debian, Redhat, and Ubuntu, you may have missing dependencies. Use the following commands to install these dependencies depending on your version of Linux:
   

   **Debian:** 
   ```bash
   sudo apt-get install libuwind8
   ```

   **Redhat:** 
   ```bash
   yum install libXScrnSaver
   ```

   **Ubuntu:** 
   ```bash
   sudo apt-get install libxss1

   sudo apt-get install libgconf-2-4

   sudo apt-get install libunwind8
   ```


## Uninstall SQL Operations Studio (preview)

If you installed [!INCLUDE[name-sos-short](../includes/name-sos-short.md)] using the Windows installer, then uninstall the same way you remove any Windows application.

If you installed [!INCLUDE[name-sos-short](../includes/name-sos-short.md)] with a .zip or other archive, then simply delete the files.

## Supported Operating Systems

[!INCLUDE[name-sos](../includes/name-sos-short.md)] runs on Windows, macOS, and Linux, and is supported on the following platforms:

### Windows
- Windows 10 (64-bit)
- Windows 8.1 (64-bit)
- Windows 8 (64-bit)
- Windows 7 (SP1) (64-bit) - Requires [KB2533623](https://www.microsoft.com/en-us/download/details.aspx?id=26767)
- Windows Server 2016
- Windows Server 2012 R2 (64-bit)
- Windows Server 2012 (64-bit)
- Windows Server 2008 R2 (64-bit)

### macOS
- macOS 10.13 High Sierra
- macOS 10.12 Sierra

### Linux
- Red Hat Enterprise Linux 7.4
- Red Hat Enterprise Linux 7.3
- SUSE Linux Enterprise Server v12 SP2
- Ubuntu 16.04

## Check for updates
To check for latest updates, click the gear icon on the bottom left of the window and click **Check for Updates**

## Next Steps

See one of the following quickstarts to get started:
- [Connect & Query SQL Server](quickstart-sql-server.md)
- [Connect & Query Azure SQL Database](quickstart-sql-database.md)
- [Connect & Query Azure Data Warehouse](quickstart-sql-dw.md)

Contribute to [!INCLUDE[name-sos](../includes/name-sos-short.md)]:
- [https://github.com/Microsoft/sqlopsstudio](https://github.com/Microsoft/sqlopsstudio) 

[Microsoft Privacy Statement](https://go.microsoft.com/fwlink/?LinkId=521839) and [usage data collection](usage-data-collection.md).
