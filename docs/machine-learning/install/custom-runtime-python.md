---
title: Install Python custom runtime
description: Learn how to install a Python custom runtime for SQL Server using Language Extensions.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 11/30/2020
ms.topic: how-to
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
zone_pivot_groups: python-custom-runtime-platform
monikerRange: ">=sql-server-ver15||>=sql-server-linux-ver15"
---
# Install a Python custom runtime for SQL Server
[!INCLUDE [SQL Server 2019 and later](../../includes/applies-to-version/sqlserver2019.md)]

This article describes how to install a Python custom runtime for running external Python scripts with SQL Server. The custom runtime uses the [SQL Server Language Extensions](../../language-extensions/language-extensions-overview.md).

The Python custom runtime allows you to use your own version of the Python runtime with SQL Server, instead of the default runtime version installed with [SQL Server Machine Learning Services](../sql-server-machine-learning-services.md).

::: zone pivot="python-custom-runtime-windows"

> [!NOTE]
> This article describes how to install a custom runtime for Python on Windows. To install on Linux, see [Install a Python custom runtime for SQL Server on Linux](custom-runtime-python.md?view=sql-server-linux-ver15&preserve-view=true).

## Pre-install checklist for Windows

Before installing a custom Python runtime, install the following:

+ [Cumulative Update (CU) 3 or later for SQL Server 2019](../../database-engine/install-windows/latest-updates-for-microsoft-sql-server.md).

+ [Python 3.7](https://www.python.org/downloads/).

    The Python language extension used for the custom Python runtime currently support Python 3.7 only. If you would like to use a different version of Python, follow the instruction in the [Python Language Extension GitHub repo](https://github.com/microsoft/sql-server-language-extensions/tree/master/language-extensions/python) to modify and rebuild the extension.

    During the installation of Python, add Python to the PATH.

    ![Add Python 3.7 to path.](../install/media/python-379.png)

## Add SQL Server Language Extensions for Windows

> [!NOTE]
> If you have Machine Learning Services installed on SQL Server 2019, the extensibility framework is already installed and you can skip this step.

Follow the steps below to install [Language Extensions](../../language-extensions/language-extensions-overview.md), which is used for the custom Python runtime.

1. Start the setup wizard for SQL Server 2019.
  
1. On the **Installation** tab, select **New SQL Server stand-alone installation or add features to an existing installation**.
    
1. On the **Feature Selection** page, select these options:
  
    - **Database Engine Services**
  
        To use Language Extensions with SQL Server, you must install an instance of the database engine. You can use either a default or a named instance.
  
    - **Machine Learning Services and Language Extensions**
   
       Select **Machine Learning Services and Language Extensions**. Do not select Python, as you will be installing the custom Python runtime later.

    ![SQL Server 2019 CU3 or later installation features](../install/media/sql-feature-selection.png) 

1. On the **Ready to Install** page, verify that these selections are included, and select **Install**.
  
    + Database Engine Services
    + Machine Learning Services and Language Extensions

1. After the setup is complete, restart the computer if you're ask to do so. 

## Install pandas

Install the [pandas](https://pandas.pydata.org/) package for Python from an *elevated* command prompt:

```bash
python.exe -m pip install pandas
```

## Update the system environment variables

Add or modify PYTHONHOME as a system environment variable.

+ In the Windows search box, type "environment" and select **Edit the system environment variables**.
+ In the **Advanced** tab, select **Environment Variables**.
+ Under **System variables**, select **New** to create PYTHONHOME to point to the Python 3.7 installation location.
If PYTHONHOME already exists, select **Edit** to point it to the Python 3.7 installation location.
+ Select **OK** to close remaining windows.

![Create PYTHONHOME system variable.](../install/media/sys-pythonhome.png)

## Grant access to the custom Python installation folder

Run the following **icacls** commands from a new *elevated* command prompt to grant READ & EXECUTE access to PYTHONHOME to **SQL Server Launchpad Service** and SID **S-1-15-2-1** (**ALL_APPLICATION_PACKAGES**). The launchpad service username is: `NT Service\MSSQLLAUNCHPAD$INSTANCENAME* where INSTANCENAME` is the instance name of your SQL Server. The commands will recursively grant access to all files and folders under the given directory path.

Append the instance name to `MSSQLLAUNCHPAD` (`MSSQLLAUNCHPAD$INSTANCENAME`). In this example, INSTANCENAME is the default instance `MSSQLSERVER`.

1. Give permissions to **SQL Server Launchpad Service user name**.

    ```cmd
    icacls "%PYTHONHOME%" /grant "NT Service\MSSQLLAUNCHPAD":(OI)(CI)RX /T
    ```

    > [!NOTE]
    > For named instance, the command will be `icacls "%PYTHONHOME%" /grant "NT Service\MSSQLLAUNCHPAD$SQL01":(OI)(CI)RX /T`for an instance called **SQL01**.

2. Give permissions to **SID S-1-15-2-1**.

    ```cmd
    icacls "%PYTHONHOME%" /grant *S-1-15-2-1:(OI)(CI)RX /T
    ```

    > [!NOTE]
    > The preceding command grants permissions to the computer **SID S-1-15-2-1**, which is equivalent to **ALL APPLICATION PACKAGES** on an English version of Windows. Alternatively, you can use `icacls "%R_HOME%" /grant "ALL APPLICATION PACKAGES":(OI)(CI)RX /T` on an English version of Windows.

## Restart SQL Server Launchpad service

From an *elevated* command prompt, run the following commands. Replace "MSSQLSERVER" with the name of your SQL Server instance.

```CMD
net stop MSSQLLAUNCHPAD$MSSQLSERVER
net start MSSQLLAUNCHPAD$MSSQLSERVER
```

Alternatively, right-click the SQL Server Launchpad service in the **Services** app of the system and click the **Restart** command. Or use [SQL Server Configuration Manager](../../relational-databases/sql-server-configuration-manager.md) to restart the service.

## Download Python language extension

Download the [zip file containing the Python language extension for Windows](https://github.com/microsoft/sql-server-language-extensions/releases). Recommended to use the release version in production. Use the debug version in development or test since it provides verbose logging information to investigate any errors.

## Register external language on Windows

Register this Python language extension with [CREATE EXTERNAL LANGUAGE](../../t-sql/statements/create-external-language-transact-sql.md) for each database you want to use it in. Use [Azure Data Studio](../../azure-data-studio/download-azure-data-studio.md) to connect to SQL Server and run the following T-SQL command. Modify the path in this statement to reflect the location of the downloaded language extension zip file (python-lang-extension.zip).

> [!NOTE]
> **Python** is a reserved word and can't be used as the name for a new external language name. Use a different name instead. For example, you can use **myPython**.

```sql
CREATE EXTERNAL LANGUAGE [myPython]
FROM (CONTENT = N'/path/to/python-lang-extension.zip', FILE_NAME = 'pythonextension.dll');
GO
```

::: zone-end

::: zone pivot="python-custom-runtime-linux"

You can install SQL Server on Red Hat Enterprise Linux (RHEL), SUSE Linux Enterprise Server (SLES), and Ubuntu. For more information, see [the Supported platforms section in the Installation guidance for SQL Server on Linux](../../linux/sql-server-linux-setup.md).

> [!NOTE]
> This article describes how to install a custom runtime for Python on Linux. To install on Windows, see the [Install a Python custom runtime for SQL Server on Windows](custom-runtime-python.md?view=sql-server-ver15&preserve-view=true)

## Pre-install checklist on Linux

Before installing a Python custom runtime, install the following:

+ [SQL Server 2019 for Linux (Cumulative Update 3 or later)](../../linux/sql-server-linux-setup.md).
When you install SQL Server on Linux, you must configure a Microsoft repository. For more information, see [configuring repositories](../../linux/sql-server-linux-change-repo.md)

  > [!NOTE]
  > Python custom runtime requires Cumulative Update (CU) 3 or later for SQL Server 2019.

+ [SQL Server Language Extensions on Linux with the extensibility framework](../../linux/sql-server-linux-setup-language-extensions-java.md).

+ [Python 3.7](https://www.python.org/downloads/release/python-379/).

## Add SQL Server Language Extensions for Linux

> [!NOTE]
> If you have Machine Learning Services installed on SQL Server 2019, the **mssql-server-extensibility** package for language extensions is already installed and you can skip this step.

Language Extensions use the extensibility framework for executing external code. Code execution is isolated from the core engine processes, but fully integrated with SQL Server query execution.

Use the following commands to install Language Extensions, depending on your version of Linux.

### Ubuntu
> [!TIP]
> If possible, `update` to refresh packages on the system prior to installation. Ubuntu might not have the https apt transport option. To install it, use `apt-get install apt-transport-https`.

```bash
# Install as root or sudo
sudo apt-get install mssql-server-extensibility
```

### Red Hat
```bash
# Install as root or sudo
sudo yum install mssql-server-extensibility
```

### Suse
```bash
# Install as root or sudo
sudo zypper install mssql-server-extensibility
```

## Install Python 3.7 and pandas

Install Python 3.7, the libpython3.7 library, and the pandas package. 

The following are example commands for Ubuntu:

```bash
# Install python3.7 and the corresponding library:
sudo add-apt-repository ppa:deadsnakes/ppa
sudo apt-get update
sudo apt-get install python3.7 python3-pip libpython3.7

# Install pandas to /usr/lib:
sudo python3.7 -m pip install pandas -t /usr/lib/python3.7/dist-packages
```

## Using a custom installation of Python 3.7

> [!NOTE]
> If you have installed Python in the default location of `/usr/lib/python3.7`, you can skip to [the next section](#download-python-linux).

If you built your own version of Python 3.7, use the following commands so that SQL Server can find and load your custom installation.

### Update the environment variables

1. Edit the mssql-launchpadd service to add the PYTHONHOME environment variable to the file `/etc/systemd/system/mssql-launchpadd.service.d/override.conf`

      ```bash
      sudo systemctl edit mssql-launchpadd
      ```

    + Insert the following text in the `/etc/systemd/system/mssql-launchpadd.service.d/override.conf` file that opens. Set value of PYTHONHOME to the custom Python installation path.

      ```vi
      [Service]
      Environment="PYTHONHOME=/path/to/installation/of/python3.7"
      ```

    + Save and close.

2. Make sure `libpython3.7m.so.1.0` can be loaded.

    + Create a custom-python.conf file in `/etc/ld.so.conf.d`.

      ```bash
      sudo vi /etc/ld.so.conf.d/custom-python.conf
      ```

    + In the file that opens, add the path to **libpython3.7m.so.1.0** from the custom Python installation.

      ```vi
      /path/to/installation/of/python3.7/lib
      ```

    + Save and close the new file.

    + Run `ldconfig` and verify `libpython3.7m.so.1.0` can be loaded by running the following commands and checking that all the dependent libraries can be found.

      ```bash
      sudo ldconfig
      ldd /path/to/installation/of/python3.7/lib/libpython3.7m.so.1.0
      ```

### Grant access to the custom Python folder

Set the `datadirectories` option in the extensibility section of /var/opt/mssql/mssql.conf file to the custom python installation.

```bash
sudo /opt/mssql/bin/mssql-conf set extensibility.datadirectories /path/to/installation/of/python3.7
```

### Restart the mssql-launchpadd service

```bash
sudo systemctl restart mssql-launchpadd
```
## <a name="download-python-linux"></a> Download Python language extension

Download the [zip file containing the Python language extension for Linux](https://github.com/microsoft/sql-server-language-extensions/releases). Recommended to use the release version in production. Use the debug version in development or test since it provides verbose logging information to investigate any errors.

## Register external language on Linux

Register this Python language extension with [CREATE EXTERNAL LANGUAGE](../../t-sql/statements/create-external-language-transact-sql.md) for each database you want to use it in. Use [Azure Data Studio](../../azure-data-studio/download-azure-data-studio.md) to connect to SQL Server and run the following T-SQL command. 
Modify the path in this statement to reflect the location of the downloaded language extension zip file (python-lang-extension.zip).

> [!NOTE]
> **Python** is a reserved word and can't be used as the name for a new external language name. Use a different name instead. For example, you can use **myPython**.

```sql
CREATE EXTERNAL LANGUAGE myPython 
FROM (CONTENT = N'/PATH/TO/python-lang-extension.zip', FILE_NAME = 'libPythonExtension.so.1.0');
GO
```

::: zone-end

## Enable external script execution in SQL Server

An external script in Python can be executed via the stored procedure [sp_execute_external script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) run against SQL Server.

To enable external scripts, execute the following SQL commands using [Azure Data Studio](../../azure-data-studio/download-azure-data-studio.md), connected to SQL Server.

```sql
sp_configure 'external scripts enabled', 1;
RECONFIGURE WITH OVERRIDE;  
```

## Verify language extension installation

This SQL script tests the functionality of the installed language extension.

```sql
EXEC sp_execute_external_script
@language =N'myPython',
@script=N'
import sys
print(sys.path)
print(sys.version)
print(sys.executable)'
```

## Next steps

+ [Extensibility framework in SQL Server](../concepts/extensibility-framework.md)
+ [Language Extensions Overview](../../language-extensions/language-extensions-overview.md)
