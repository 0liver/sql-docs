---
title: How to use distributed transactions with SQL Server on Docker | Microsoft Docs
description: This article explains how to use Dprovides a walk-through for configuring MSDTC on Linux.
author: rothja
ms.author: jroth
manager: craigg
ms.date: 09/24/2018
ms.topic: conceptual
ms.prod: sql
ms.component: ""
ms.suite: "sql"
ms.custom: "sql-linux"
ms.technology: linux
monikerRange: ">= sql-server-ver15 || = sqlallproducts-allversions"
---

# How to use distributed transactions with SQL Server on Docker

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-linuxonly](../includes/appliesto-ss-xxxx-xxxx-xxx-md-linuxonly.md)]

This article explains how to set up SQL Server on Docker for distributed transactions.

Starting with SQL Server 2019 preview, container images support the Microsoft Distributed Transaction Coordinator (MSDTC) required for distributed transactions. To understand the communications requirements for MSDTC, see [How to configure the Microsoft Distributed Transaction Coordinator (MSDTC) on Linux](sql-server-linux-configure-msdtc.md). This article explains the special requirements and scenarios related to SQL Server Docker containers.

## Configuration

To enable MSDTC transaction in containers for docker, you must set two new environment variables:

- **MSSQL_RPC_PORT**: the TCP port that RPC endpoint mapper service binds to and listens on.  
- **MSSQL_DTC_TCP_PORT**  the port that MSDTC service is configured to listen on.

### Pull and run

The following example shows how to use these environment variables to pull and run a container configured for MSDTC. This allows it to communicate with any application on any hosts.

```bash
docker run \
   -e 'ACCEPT_EULA=Y' -e 'MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>' \
   -e 'MSSQL_RPC_PORT=13500' -e 'MSSQL_DTC_TCP_PORT=51000' \
   -p 51433:1433 -p 13500:13500 -p 51000:51000  \
   -d mcr.microsoft.com/mssql/server/mssql-server-linux:vNext-CTP2.0
```

The following command shows the same command for Docker on Windows in PowerShell:

```PowerShell
docker run `
   -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" `
   -e "MSSQL_RPC_PORT=13500" -e "MSSQL_DTC_TCP_PORT=51000" `
   -p 51433:1433 -p 13500:13500 -p 51000:51000 `
   -d "mcr.microsoft.com/mssql/server/mssql-server-linux:vNext-CTP2.0"
```

In this command, the **RPC Endpoint Mapper** service has been bound to port 13500, and the **MSDTC** service has been bound to port 51000 within docker virtual network. SQL Server TDS communication occurs on port 1433 within docker virtual network. These ports have been externally exposed to host as TDS port 51433, RPC endpoint mapper port 13500, and MSDTC port 51000.

> [!NOTE]
> The RPC Endpoint Mapper and MSDTC port do not have to be same on host and the container. So while RPC Endpoint Mapper port was configured to be 13500 on container, it could potentially be mapped to port 13501 or any other available port on the host server.

## Configure the firewall

In order to communicate with and through the host, you must also configure the firewall on the host server for the containers. Open the firewall for all ports that docker container exposes for external communication. In the previous example, this would be ports 135, 51433, and 51000. These are the ports on the host itself and not the ports they map to in the container. So, if RPC Endpoint mapper port 51000 of container was mapped to host port 51001, then port 51001 (not 51000) should be opened in the firewall for communication with the host.  

The following example shows how to create these rules on Ubuntu.

```bash
sudo ufw allow from any to any port 51433 proto tcp
sudo ufw allow from any to any port 51000 proto tcp
sudo ufw allow from any to any port 135 proto tcp
```

The following example shows how this could be done on Red Hat Enterprise Linux (RHEL):

```bash
sudo firewall-cmd --zone=public --add-port=51999/tcp --permanent
sudo firewall-cmd --zone=public --add-port=51433/tcp --permanent
sudo firewall-cmd --zone=public --add-port=135/tcp --permanent
sudo firewall-cmd --reload
```

## Configure port routing on the host

If the docker container participates in distributed transactions with a server external to the host, then the host must configure port routing. For more information, see [Configure port routing](sql-server-linux-configure-msdtc.md#configure-port-routing).

## Next steps

For more information about MSDTC on Linux, see [How to configure the Microsoft Distributed Transaction Coordinator (MSDTC) on Linux](sql-server-linux-configure-msdtc.md).