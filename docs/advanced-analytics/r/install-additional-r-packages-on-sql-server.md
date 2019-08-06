---
title: Install new R language packages using standard R tools
description: Add new R packages to SQL Server Machine Learning Services or SQL Server R Services
ms.prod: sql
ms.technology: machine-learning

ms.date: 06/13/2019
ms.topic: conceptual
author: garyericson
ms.author: garye
ms.reviewer: davidph
monikerRange: ">=sql-server-2016||>=sql-server-linux-ver15||=sqlallproducts-allversions"
---

# Install new R packages on SQL Server Machine Learning Services or SQL Server R Services

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]

This article describes how to use standard R tools to install new R packages to an instance of SQL Server Machine Learning Services or SQL Server R Services. You can install packages on a SQL Server that has an Internet connection, as well as one that is isolated from the Internet.

In addition to standard R tools, you can install R packages using:

::: moniker range="=sql-server-2017||=sqlallproducts-allversions"
+ [T-SQL](install-r-packages-tsql.md) (CREATE EXTERNAL LIBRARY)
::: moniker-end
+ [RevoScaleR](use-revoscaler-to-manage-r-packages.md)

## General considerations

+ R code running in SQL Server can use only packages installed in the default instance library. SQL Server cannot load packages from external libraries, even if that library is on the same computer.
The R package library is located in the Program Files folder of your SQL Server instance, and by default, installing in this folder requires administrator permissions.
For more information, see [Package library location](../package-management/installed-package-information.md#package-library-location).

+ Non-administrators can install packages using RevoScaleR 9.0.1 and later, or using CREATE EXTERNAL LIBRARY. The **dbo_owner** user, or a user with CREATE EXTERNAL LIBRARY permission, can install R packages to the current database. For more information, see:
  + [How to use RevoScaleR functions to find or install R packages on SQL Server](use-revoscaler-to-manage-r-packages.md)
  + [Use T-SQL (CREATE EXTERNAL LIBRARY) to install R packages on SQL Server](install-r-packages-tsql.md)

+ R features are included in several Microsoft products, all of which could co-exist on the same computer. If you install one or more of these products, your computer will have separate installations of R for each, with duplicates of all the R tools and libraries. However, only packages that are installed in the R_SERVICES library folder can be used in-database on SQL Server.
For more information about the R_SERVICES folder, see [Package library location](../package-management/installed-package-information.md#package-library-location).

+ On a hardened SQL Server environment, you might want to avoid the following:
  + Packages that require network access
  + Packages that require elevated file system access
  + Packages used for web development or other tasks that don't benefit by running inside SQL Server

## Online installation (with Internet access)

If the SQL Server has access to the Internet, then you can use standard package installation tools to install R packages.

For example, the following procedure uses RGui:

1. Determine the location of the instance library (see [Get R package information](../package-management/default-packages.md)) and navigate to the folder where the R tools are installed. For example the default path for a SQL Server default instance is:

   ::: moniker range="=sql-server-2016||=sqlallproducts-allversions"
   `C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\R_SERVICES\bin\x64\Rgui.exe`
   ::: moniker-end

   ::: moniker range="=sql-server-2017||=sqlallproducts-allversions"
   `C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER\R_SERVICES\bin\x64\Rgui.exe`
   ::: moniker-end

   ::: moniker range=">sql-server-2017||=sqlallproducts-allversions"
   `C:\Program Files\Microsoft SQL Server\MSSQL15.MSSQLSERVER\R_SERVICES\bin\x64\Rgui.exe`
   ::: moniker-end

1. Right-click **RGui.exe** and select **Run as administrator**.

1. Run the R command `install.packages` and specify the package name. If the package has any dependencies, the installer automatically downloads the dependencies and installs them.

If you have multiple, side-by-side instances of SQL Server, run installation separately for each instance in which you want to use the package. Packages cannot be shared across instances.

## <a name = "bkmk_offlineInstall"></a> Offline installation (no internet access)

Frequently, servers that host production databases don't have an internet connection. To install R packages in that environment, you download and prepare packages and dependencies in advance (as zipped files), and then copy the files to a folder on the server. Once the files are in place, the packages can be installed offline.

Identifying all dependencies gets complicated. For R, we recommend that you use [**miniCRAN**](https://andrie.github.io/miniCRAN/) to create a local repository.
**miniCRAN** takes a list of packages you want to install, analyzes dependencies, and collects all the necessary zipped files. It then creates a single repository that you can copy to the isolated SQL Server instance. The [**igraph**](https://igraph.org/r/) package is also helpful in analyzing package dependencies.

For more information, see [Create a local R package repository using miniCRAN](create-a-local-package-repository-using-minicran.md).

Once the zip file is on the SQL Server instance, you can install it using standard R tools on the server. For example, the following procedure uses RGui:

1. Determine the location of the instance library (see [Get R and Python package information](../package-management/default-packages.md)) and navigate to the folder where the R tools are installed. For example the default path for a SQL Server default instance is:

   ::: moniker range="=sql-server-2016||=sqlallproducts-allversions"
   `C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\R_SERVICES\bin\x64\Rgui.exe`
   ::: moniker-end

   ::: moniker range="=sql-server-2017||=sqlallproducts-allversions"
   `C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER\R_SERVICES\bin\x64\Rgui.exe`
   ::: moniker-end

   ::: moniker range=">sql-server-2017||=sqlallproducts-allversions"
   `C:\Program Files\Microsoft SQL Server\MSSQL15.MSSQLSERVER\R_SERVICES\bin\x64\Rgui.exe`
   ::: moniker-end

1. Right-click **RGui.exe** and select **Run as administrator**.

1. Run the R command `install.packages` and specify the package or repository name, and the location of the zipped files. For example:

   ```R
   install.packages("C:\\Temp\\Downloaded packages\\mynewpackage.zip", repos=NULL)
   ```

   This command extracts the R package `mynewpackage` from its local zipped file and installs the package. If the package has any dependencies, the installer checks for existing packages in the library. If you have created a repository that includes the dependencies, the installer installs the required packages as well.

   > [!NOTE]
   > If any required packages are not present in the instance library, and cannot be found in the zipped files, installation of the target package fails.

As an alternative to **miniCRAN**, you can perform these steps manually:

1. Identify all package dependencies.
1. Check whether any required packages are already installed on the server. If the package is installed, verify that the version is correct.
1. Download the package and all dependencies to a separate computer with Internet access.
1. Place the package and dependencies in a single package archive.
1. Zip the archive if it's not already in zipped format.
1. Move the files to a folder accessible by the server.
1. Run a supported installation command or DDL statement to install the package into the instance library.

## See also

+ [Install new Python packages](../python/install-additional-python-packages-on-sql-server.md)
+ [Tutorials, samples, solutions](../tutorials/machine-learning-services-tutorials.md)
