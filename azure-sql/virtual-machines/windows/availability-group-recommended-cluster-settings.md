---
title: Recommended cluster settings for availability group
description: Learn how to modify your cluster settings to better support an availability group for a SQL Server on Azure VM. 
services: virtual-machines
documentationCenter: na
author: MashaMSFT
editor: monicar
tags: azure-service-management

ms.assetid: 601eebb1-fc2c-4f5b-9c05-0e6ffd0e5334
ms.service: virtual-machines-sql
ms.subservice: hadr

ms.topic: overview
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: "03/03/2021"
ms.author: mathoma

---

# Recommended cluster settings for availability group
[!INCLUDE[appliesto-sqlvm](../../includes/appliesto-sqlvm.md)]

If above actions don't result in improved performance such in the scenario where you're unable to move to a VM or disks with higher limit because of financial or other constraints, you can opt for relaxed monitoring of the SQL Always On AG/FCI. It will mask the underlying problem only and these are only temporary solutions and reduces (not eliminates) the likelihood of a failure. You might need to do trial and error to find the best values for your environment.

## Always on parameters for relaxed monitoring    

Modify the Always on AG/FCI parameters to achieve relaxed monitoring: 

|    AG/FCI parameters                           |
|------------------------------------------------|
|**Lease timeout**                               |
|Prevents split-brain.                           |
| Default: 20000                                 |
| **Healthcheck timeout**                        |
| Determines health of the Primary replica.      |
| Default: 30000                                 |
| **Failure-Condition Level**                    |
| Conditions that trigger an automatic failover. |

If you're concerned about primary and secondary replica connectivity timeout, you can review following parameter:

|  Session timeout                                |
|-------------------------------------------------| 
|Check communication issue between Primary and Secondary |
|Default 10 seconds|

## Constraints to follow
  
Few things to consider before making any changes:

|    Constraints                           |
|------------------------------------------------|
|    It isn't advised to lower any timeout values below their default values.                                          |
|SameSubnetThreshold <= CrossSubnetThreshold       |
|SameSubnetDelay <= CrossSubnetDelay                           |
|The lease interval (½ * LeaseTimeout) must be shorter than SameSubnetThreshold * SameSubnetDelay                                              | 

## Lease timeout

Lease timeout prevents split brain scenario. On an Azure VM this can happen if there's resource contention like slow IO, high CPU, or low memory. Here are some guidelines to relax the health timeout, if the underlying conditions cannot be avoided easily.  

| Message ID | Description |
|-|-|
| 35201 | A connection timeout has occurred while attempting to establish a connection to availability replica 'replicaname' with id [availability_group_id]. Either a networking or firewall issue exists, or the endpoint address provided for the replica isn't the database mirroring endpoint of the host server instance. |
| 35206 | A connection timeout has occurred on a previously established connection to availability replica 'replicaname' with id [availability_group_id]. Either a networking or a firewall issue exists, or the availability replica has transitioned to the resolving role. |

 ## How to relax lease timeout

The lease mechanism is controlled by a single value specific to each AG in a WSFC cluster. To navigate to this value in Failover Cluster Manager:
1. In the roles tab, find the target AG role. Select on the target AG role.
2. Right-click the AG resource at the bottom of the window and select Properties.
3. In the popup window, navigate to the properties tab and there will be a list of values specific to this AG. Select the LeaseTimeout value to change it.
   
## Health-check timeout threshold

* WSFC resource DLL of the availability group does a health check of the primary replica by calling the sp_server_diagnostics stored procedure on the instance of SQL Server that hosts the primary replica.


* sp_server_diagnostics returns results at an interval that equals 1/3 of the health-check timeout threshold for the availability group. 

* The default health-check timeout threshold is 30 seconds, which causes sp_server_diagnostics to return at a 10-second interval.

 If sp_server_diagnostics is slow or isn't returning information, the resource DLL will wait for the full interval of the health-check timeout threshold before determining that the primary replica is unresponsive. 

If the primary replica is unresponsive, an automatic failover is initiated, if currently supported.
## Relax health-check timeout

In order to Relax Health-Check Timeout to 60,000-millisecond following command can be used
```sql
ALTER AVAILABILITY GROUP AG1 SET (HEALTH_CHECK_TIMEOUT =60000);
```
## Failure-condition level 
The failure-condition level specifies what failure conditions trigger an automatic failover. There are five failure-condition levels, which range from the least restrictive (level 1) to the most restrictive (level 5). A given level encompasses the less restrictive levels. Thus, the strictest level, five, includes the four less restrictive conditions, and so forth. You can set the FAILURE_CONDITION_LEVEL to less restrictive value from default of 3 when you can't resolve the underlying issue easily. Following command sets failure condition to the least restrictive level of 1 for the availability group named AG1.
```sql  
ALTER AVAILABILITY GROUP AG1 SET (FAILURE_CONDITION_LEVEL = 1);
```
## Session timeout

The session-timeout period is a replica property that controls how long (in seconds) that an availability replica waits for a ping response from a connected replica before considering the connection to have failed. By default, a replica waits 10 seconds for a ping response. This replica property applies only the connection between a given secondary replica and the primary replica of the availability group.

We recommend that you keep the time-out period at 10 seconds or greater.  For Synchronous replica, changing session-timeout to high value can increase HADR_Sync_commit waits. 

The following example, entered on the primary replica of the AccountsAG availability group, changes the session-timeout value to 15 seconds for the replica located on the INSTANCE09 server instance.
```sql
ALTER AVAILABILITY GROUP AccountsAG   
   MODIFY REPLICA ON 'INSTANCE09' WITH (SESSION_TIMEOUT = 15);  
```
## Known issues

If the Windows cluster settings are too aggressive for your environment, you may see following message in the system event log frequently. To get more information, please review [Troubleshooting cluster issue with Event ID 1135.](https://docs.microsoft.com/windows-server/troubleshoot/troubleshooting-cluster-event-id-1135)

| Event ID        | Description                                                            |
|----|----|
|       1135      |  Cluster node 'Node1' was removed from the active failover cluster membership. The Cluster service on this node may have stopped. This could also be due to the node having lost communication with other active nodes in the failover cluster. Run the Validate a Configuration wizard to check your network configuration. If the condition persists, check for hardware or software errors related to the network adapters on this node. Also check for failures in any other network components to which the node is connected such as hubs, switches, or bridges.|

If the SQL AG/FCI monitoring process is too aggressive, you may see frequent AG/FCI restarts, failures, or failovers. For AG, additionally you can see following messages in the SQL Server: 

| Message ID | Description                                                                   |
|--|--|
| 19407| The lease between availability group ‘PRODAG’ and the Windows Server Failover Cluster has expired. A connectivity issue occurred between the instance of SQL Server and the Windows Server Failover Cluster. To determine whether the availability group is failing over correctly, check the corresponding availability group resource in the Windows Server Failover Cluster |
| 19419| The renewal of the lease between availability group ‘%.*ls’ and the Windows Server Failover Cluster failed because the existing lease is no longer valid.   |

If the session timeout is too aggressive for your environment, you may see following messages frequently:

| Message ID | Description |
|-|-|
| 35201 | A connection timeout has occurred while attempting to establish a connection to availability replica 'replicaname' with id [availability_group_id]. Either a networking or firewall issue exists, or the endpoint address provided for the replica is not the database mirroring endpoint of the host server instance. |
| 35206 | A connection timeout has occurred on a previously established connection to availability replica 'replicaname' with id [availability_group_id]. Either a networking or a firewall issue exists, or the availability replica has transitioned to the resolving role. 

## Relax monitoring on certain periods of time
In some scenario, it might be beneficial to opt for more relaxed monitoring on certain period of time of in certain environments. One example can be an environment might see cluster or AG/FCI failures only during maintenance hours due to increased IO loads resulting in Azure VM or disk IO throttling. Some examples of increased IOs can be: 
* Database backup 
* Index maintenance  
* Update statistics  
* DBCC CHECKDB  
* VM backup 

Ideal solution to avoid these IO would be going for lager VM or disk size, if the IO load cannot be spread out or reduced. But if higher size isn't an acceptance solution, having an even more relaxed environment might be considered. Here are the steps can be followed to achieve this:
* Just before high resource consuming activity set the monitoring for Windows cluster (and if needed Always On AG/FCI) to more relaxed so that possibility of a failure is minimized. 
* Complete the activity. 
* Change the monitoring setting back to what is needed during normal business operation. 
* Use SQL agent jobs to achieve this. A hard failure during a more relaxed environment will take longer to detect. 

> [!Note]
> Changing the cluster threshold parameters or AG/FCI timeouts/Failure condition level will take effect immediately, you don't have to restart the cluster or any resources.
