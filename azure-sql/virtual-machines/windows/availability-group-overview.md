---
title: Overview of SQL Server Always On availability groups
description: This article introduces SQL Server Always On availability groups on Azure Virtual Machines.
services: virtual-machines
documentationCenter: na
author: MashaMSFT
editor: monicar
tags: azure-service-management

ms.assetid: 601eebb1-fc2c-4f5b-9c05-0e6ffd0e5334
ms.service: virtual-machines-sql

ms.topic: overview
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: "10/07/2020"
ms.author: mathoma
ms.custom: "seo-lt-2019, devx-track-azurecli"

---

# Always On availability group on SQL Server on Azure VMs
[!INCLUDE[appliesto-sqlvm](../../includes/appliesto-sqlvm.md)]

This article introduces Always On availability groups on SQL Server on Azure Virtual Machines (VMs). 

## Overview

Always On availability groups on Azure Virtual Machines are similar to [Always On availability groups on-premises](/sql/database-engine/availability-groups/windows/always-on-availability-groups-sql-server). However, since the virtual machines are hosted in Azure, there are a few additional considerations as well, such as VM redundancy, and routing traffic on the Azure network. 

The following diagram illustrates the parts of a complete SQL Server Availability Group in Azure Virtual Machines.

![Availability Group](./media/availability-group-overview/00-EndstateSampleNoELB.png)


## VM redundancy 

To increase redundancy and high availability, the SQL Server VMs should either be in the same [availability set](../../../virtual-machines/windows/tutorial-availability-sets.md#availability-set-overview), or different [availability zones](/azure/availability-zones/az-overview).

An availability set is a grouping of resources which are configured such that no two land in the same availability zone. This prevents impacting multiple resources in the group during deployment roll outs. To be supported in Azure, SQL Server VMs participating in an availability group must be in the same availability set or two different availability zones. 

## Connectivity 

In a traditional on-premises deployment, clients connect to the availability group listener using the virtual network name (VNN), and the listener routes traffic to the appropriate SQL Server replica in the availability group. However, in Azure, there is an extra requirement to route traffic on the Azure network. 

With SQL Server on Azure VMs, configure a [load balancer](hadr-vnn-azure-load-balancer.md) to route traffic to your availability group listener, or, if you're on SQL Server 2019 CU8 and above, configure a [distributed network name (DNN) listener](hadr-distributed-network-name-dnn-configure.md) to replace the traditional availability group listener. 


### Load balancer 

Use an [Azure Load Balancer](../../../load-balancer/load-balancer-overview.md) to route traffic from the client to the availability group listener on the Azure network. 

The load balancer holds the IP addresses for the availability group listener. If you have more than one availability group, each group requires a listener. One load balancer can support multiple listeners.

To get started, see [configure a load balancer](availability-group-vnn-azure-load-balancer.md). 

### DNN listener

SQL Server 2019 CU8 introduces support for the distributed network name (DNN) listener. The DNN listener replaces the traditional availability group listener, negating the need for an Azure Loud Balancer to route traffic on the Azure network. 

The DNN listener is the recommended HADR connectivity solution in Azure as it simplifies deployment, reduces maintenance and cost, and reduces failover time in the event of a failure. 

Use the DNN listener to replace an existing VNN listener, or alternatively, use it in conjunction with an existing VNN listener so that your availability group has two distinct connection points - one using the VNN listener name (and port if non-default), and one using the DNN listener name and port. 

To get started, see [configure a DNN listener](availability-group-distributed-network-name-dnn-listener-configure.md).


## Deployment 

There are multiple options for deploying an availability group to SQL Server on Azure VMs, some with more automation than others. The following table provides a comparison of the options available: 


|  | Windows Server Version | SQL Server Version | SQL Server Edition | WSFC Quorum Config | DR with Multi-region | Multi-subnet support | Support for an existing AD | DR with multi-zone same region | Dist-AG support with no AD domain | Dist-AG support with no cluster |  
| :------ | :-----| :-----| :-----| :-----| :-----| :-----| :-----| :-----| :-----| :-----|
| **[Azure portal](availability-group-azure-portal-configure.md)** | 2019 </br> 2016 | 2019 </br>2017 </br>2016   | Ent | Cloud witness | No | Yes | Yes | Yes | No | No |
| **[Azure CLI / PowerShell](availability-group-az-cli-configure.md)** | 2019 </br> 2016 | 2019 </br>2017 </br>2016   | Ent | Cloud witness | No | Yes | Yes | Yes | No | No |
| **[Quickstart Templates](availability-group-quickstart-template-configure.md)** | 2019 </br> 2016 | 2019 </br>2017 </br>2016  | Ent | Cloud witness | No | Yes | Yes | Yes | No | No |
| **[Manual](availability-group-manually-configure-prerequisites-tutorial.md)** | All | All | All | All | Yes | Yes | Yes | Yes | Yes | Yes |


When you are ready to build a SQL Server availability group on Azure Virtual Machines, refer to these tutorials.

- [Azure Portal](availability-group-azure-portal-configure.md) 
   - Creates the cluster or lets you onboard an existing cluster for portal management 
   - Creates the availability group
   - Creates the listener with the load balancer simultaneously
   - No native DNN listener support, DNN must be created separately
- [Azure CLI / PowerShell](availability-group-az-cli-configure.md)
   - Creates the cluster
   - Does not create the availability group, you still need to do this manually
   - Creates the listener with the load balancer simultaneously
   - No native DNN listener support, DNN must be created separately
- [Quickstart Templates](availability-group-quickstart-template-configure.md)
   - Creates the cluster or lets you onboard an existing cluster for portal management 
   - Does not create the availability group, you still need to do this manually 
   - Creates the listener with the load balancer simultaneously
   - No native DNN listener support, DNN must be created separately
- [Manual](availability-group-manually-configure-prerequisites-tutorial.md)
   - Creates the cluster but does not onboard it to the SQL VM resource provider metadata
   - Creates the listener separately from the load balancer
   - No native DNN listener support, DNN must be created separately


here is another way to present this information

| |**[Azure portal](availability-group-azure-portal-configure.md)**|**[Azure CLI / PowerShell](availability-group-az-cli-configure.md)**|**[Quickstart Templates](availability-group-quickstart-template-configure.md)**|**[Manual](availability-group-manually-configure-prerequisites-tutorial.md)** | 
|---------|---------|---------|--------- |---------|
|SQL Server version |2016 + |2016 +|2016 +|2012 +|
|Windows Server version| 2016 + | 2016 + | 2016 + | All| 
|SQL Server edition |Enterprise |Enterprise |Enterprise |Enterprise, Standard|
|Creates the cluster for you|Yes|Yes | Yes |No|
|Creates the availability group for you |Yes |No|No|No|
|Creates listener and load balancer independently |No|No|No|Yes|
|Built-in DNN listener creation|No|No|No|No|
|WSFC Quorum configuration|Cloud witness|Cloud witness|Cloud witness|All|
|DR with multiple regions |No|No|No|Yes|
|Multisubnet support |Yes|Yes|Yes|Yes|
|Support for an existing AD|Yes|Yes|Yes|Yes|
|DR with multizone in the same region|Yes|Yes|Yes|Yes|
|Distributed AG with no AD |No|No|No|Yes|
|Distributed AG with no cluster |No|No|No|Yes|
||||||



## Considerations 

On an Azure IaaS VM guest failover cluster, we recommend a single NIC per server (cluster node) and a single subnet. Azure networking has physical redundancy, which makes additional NICs and subnets unnecessary on an Azure IaaS VM guest cluster. Although the cluster validation report will issue a warning that the nodes are only reachable on a single network, this warning can be safely ignored on Azure IaaS VM guest failover clusters. 

## Next steps

Review the [HADR best practices](hadr-cluster-best-practices.md) and then get started with deploying your availability group using the [Azure Portal](availability-group-azure-portal-configure.md), [Azure CLI / PowerShell](availability-group-az-cli-configure.md), [Quickstart Templates](availability-group-quickstart-template-configure.md) or [manually](availability-group-manually-configure-prerequisites-tutorial.md).

Alternatively, you can deploy a [clusterless availability group](availability-group-clusterless-workgroup-configure.md) or an availability group in [multiple regions](availability-group-configure-multiple-regions.md). 
