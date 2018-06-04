---
title: Configure a SQL Server Availability Group for read-scale on Windows | Microsoft Docs
description:
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.reviewer: ""
ms.date: 05/24/2018
ms.topic: conceptual
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ""
ms.suite: "sql"
ms.technology: high-availability
ms.tgt_pltfrm: ""
---
# Configure a SQL Server Availability Group for read-scale on Windows

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]

You can configure a SQL Server Always On Availability Group (AG) for read-scale workloads on Windows. There are two types of architectures for AGs. An architecture for high availability uses a cluster manager to provide improved business continuity and can include readable-secondary replicas. To create this high-availability architecture, see [Creation and Configuration of Availability Groups on Windows](creation-and-configuration-of-availability-groups-sql-server.md). The other architecture supports only read-scale workloads. This article explains how to create an AG without a cluster manager for read-scale workloads. This architecture provides read-scale only. It doesn't provide high availability.

>[!NOTE]
>An availability group with `CLUSTER_TYPE = NONE` can include replicas hosted on different operating system platforms. It cannot support high availability. For Linux operating system see [Configure a SQL Server Availability Group for read-scale on Linux](../../../linux/sql-server-linux-availability-group-configure-rs.md)

[!INCLUDE [Create prerequisites](../../../includes/ss-availability-group-rs-prereq.md)]

## Create the AG

Create the AG. Set `CLUSTER_TYPE = NONE`. In addition, set each replica with `FAILOVER_MODE = NONE`. Client applications running analytics or reporting workloads can directly connect to the secondary databases. You also can create a read-only routing list. Connections to the primary replica forward read connection requests to each of the secondary replicas from the routing list in a round-robin fashion.

The following Transact-SQL script creates an AG named `ag1`. The script configures the AG replicas with `SEEDING_MODE = AUTOMATIC`. This setting causes SQL Server to automatically create the database on each secondary server after it is added to the AG. Update the following script for your environment. Replace the `<node1>` and `<node2>` values with the names of the SQL Server instances that host the replicas. Replace the `<5022>` value with the port you set for the endpoint. Run the following Transact-SQL script on the primary SQL Server replica:

```sql
CREATE AVAILABILITY GROUP [ag1]
    WITH (CLUSTER_TYPE = NONE)
    FOR REPLICA ON
        N'<node1>' WITH (
            ENDPOINT_URL = N'tcp://<node1>:<5022>',
		    AVAILABILITY_MODE = ASYNCHRONOUS_COMMIT,
		    FAILOVER_MODE = MANUAL,
		    SEEDING_MODE = AUTOMATIC,
                    SECONDARY_ROLE (ALLOW_CONNECTIONS = ALL)
		    ),
        N'<node2>' WITH (
		    ENDPOINT_URL = N'tcp://<node2>:<5022>',
		    AVAILABILITY_MODE = ASYNCHRONOUS_COMMIT,
		    FAILOVER_MODE = MANUAL,
		    SEEDING_MODE = AUTOMATIC,
		    SECONDARY_ROLE (ALLOW_CONNECTIONS = ALL)
		    );

ALTER AVAILABILITY GROUP [ag1] GRANT CREATE ANY DATABASE;
```

### Join secondary SQL Servers to the AG

The following Transact-SQL script joins a server to an AG named `ag1`. Update the script for your environment. On each secondary SQL Server replica, run the following Transact-SQL script to join the AG:

```sql
ALTER AVAILABILITY GROUP [ag1] JOIN WITH (CLUSTER_TYPE = NONE);

ALTER AVAILABILITY GROUP [ag1] GRANT CREATE ANY DATABASE;
```

[!INCLUDE [Create post](../../../includes/ss-availability-group-rs-postactivity.md)]

This AG isn't a high-availability configuration. If you need high availability, follow the instructions at [Configure an Always On Availability Group for SQL Server on Linux](../../../linux/sql-server-linux-availability-group-configure-ha.md) or [Creation and Configuration of Availability Groups on Windows](creation-and-configuration-of-availability-groups-sql-server.md).

## Connect to read-only secondary replicas

There are two ways to connect to read-only secondary replicas. Applications can connect directly to the SQL Server instance that hosts the secondary replica and query the databases. They also can use read-only routing, which requires a listener.

* [Readable secondary replicas](active-secondaries-readable-secondary-replicas-always-on-availability-groups.md)
* [Read-only routing](listeners-client-connectivity-application-failover.md#ConnectToSecondary)

## Fail over the primary replica on a read-scale Availability Group

[!INCLUDE[Force failover](../../../includes/ss-force-failover-read-scale-out.md)]

## Next steps

* [Configure a distributed Availability Group](distributed-availability-groups-always-on-availability-groups.md)
* [Learn more about availability groups](overview-of-always-on-availability-groups-sql-server.md)
* [Perform a forced manual failover](perform-a-forced-manual-failover-of-an-availability-group-sql-server.md)
