---
title: Restore a database into SQL Server Big Data Cluster | Microsoft Docs
description:
author: rothja
ms.author: jroth
manager: craigg
ms.date: 10/01/2018
ms.topic: conceptual
ms.prod: sql
---

# Restore a database into the SQL Server Big Data Cluster master instance

To bring an existing SQL Server database into the master instance, we recommend using a backup, copy and restore approach.  In this example we will show how to restore the AdventureWorks database, but you can use any database backup that you have.  You can download the AdventureWorks backup [here](https://www.microsoft.com/en-us/download/details.aspx?id=49502).

First, backup your existing SQL Server database on either SQL Server on Windows or Linux using any of the usual methods of creating a database backup.

Copy the backup file to the SQL Server container in the master instance pod of the Kubernetes cluster.

```bash
kubectl cp <path to .bak file> mssql-data-pool-master-0:/tmp/ -c mssql-data-pool-data -n <name of your cluster>
```

Example:

```bash
kubectl cp ~/Downloads/AdventureWorks2016CTP3.bak mssql-data-pool-master-0:/tmp/ -c mssql-data-pool-data -n clustertest
```

Then, verify that the backup file was copied to the pod container.

```bash
kubectl exec -it mssql-data-pool-master-0 -n <name of your cluster> -c mssql-data-pool-data -- bin/bash
root@mssql-data-pool-master-0:/# ls /tmp
root@mssql-data-pool-master-0:/# exit
```

Example:

```bash
kubectl exec -it mssql-data-pool-master-0 -n clustertest -c mssql-data-pool-data -- bin/bash
root@mssql-data-pool-master-0:/# ls /tmp
```

Next, restore the database backup to master instance SQL Server.  If you are restoring a database backup that was created on Windows, you will need to get the names of the files.  In Ops Studio connected to the master instance run this SQL script:

```sql
RESTORE FILELISTONLY FROM DISK='/tmp/<db file name>.bak'
```

Example:

```sql
RESTORE FILELISTONLY FROM DISK='/tmp/AdventureWorks2016CTP3.bak'
```

![Backup file list](media/restore-database/database-restore-file-list.png)

Now, restore the database with a script like this, substituting the names/paths as needed depending on your database backup.

```sql
RESTORE DATABASE AdventureWorks2016CTP3
FROM DISK='/tmp/AdventureWorks2016CTP3.bak'
WITH MOVE 'AdventureWorks2016CTP3_Data' TO '/var/opt/mssql/data/AdventureWorks2016CTP3_Data.mdf',
        MOVE 'AdventureWorks2016CTP3_Log' TO '/var/opt/mssql/data/AdventureWorks2016CTP3_Log.ldf',
        MOVE 'AdventureWorks2016CTP3_mod' TO '/var/opt/mssql/data/AdventureWorks2016CTP3_mod'
```

Now, if you want to have your high value database be able to access data pools you will need to setup the data pool stored procedures by opening and running these scripts from the GitHub repository.

Execute the **high-value-db-configuration\data_pool_ddl_install.SQL** script.

- Setup supportability stored procedures

Execute the **high-value-db-configuration\supportability.SQL** script.
