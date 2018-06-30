---
title: High availability for SQL Server containers
description: This article introduces high availability for SQL Server containers
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.date: 6/10/2018
ms.topic: article
ms.prod: sql
ms.component: ""
ms.suite: "sql"
ms.custom: "sql-linux"
ms.technology: linux
---

# High availability for SQL Server containers

Create and manage your SQL Server instances natively in Kubernetes.

[Deploy SQL Server to docker containers](tutorial-sql-server-ag-kubernetes.md) managed by [Kubernetes](https://kubernetes.io/). In Kubernetes, a container with a SQL Server instance can automatically recover in case a cluster node fails. For more robust availability, configure SQL Server Always On availability group with SQL Server instances in containers on a Kubernetes cluster. This article compares the two solutions.

## Container with SQL Server instance on Kubernetes

Kubernetes 1.6 and later has support for [*storage classes*](http://kubernetes.io/docs/concepts/storage/storage-classes/), [*persistent volume claims*](http://kubernetes.io/docs/concepts/storage/storage-classes/#persistentvolumeclaims), and the [*Azure disk volume type*](https://github.com/kubernetes/examples/tree/master/staging/volumes/azure_disk). 

In this configuration, Kubernetes plays the role of the container orchestrator. 

![Diagram of Kubernetes SQL Server cluster](media/tutorial-sql-server-containers-kubernetes/kubernetes-sql.png)

In the preceding diagram, `mssql-server` is a SQL Server instance (container) in a [*pod*](http://kubernetes.io/docs/concepts/workloads/pods/pod/). A [replica set](http://kubernetes.io/docs/concepts/workloads/controllers/replicaset/) ensures that the pod is automatically recovered after a node failure. Applications connect to the service. In this case, the service represents a load balancer that hosts an IP address that stays the same after failure of the `mssql-server`.

Kubernetes orchestrates the resources in the cluster. When a pod or node hosting a SQL Server instance in a container fails, the orchestrator bootstraps a container in an identical pod with a SQL Server instance and attaches it to the same persistent storage.

SQL Server 2017 and later support containers on Kubernetes.

## A SQL Server Always On availability group on SQL Server containers in Kubernetes

SQL Server vNext supports availability groups on containers in a Kubernetes. For availability groups, deploy the SQL Server [Kubernetes operator](http://coreos.com/blog/introducing-operators.html) to your Kubernetes cluster. The operator helps package, deploy, and manage the availability group in a cluster.

![AG in Kubernetes Container](media/tutorial-sql-server-ag-containers-kubernetes/KubernetesCluster.png)

In the image above, a four-node kubernetes cluster hosts an availability group with three replicas. The solution includes the following components:

* A Kubernetes [*deployment*](http://kubernetes.io/docs/concepts/workloads/controllers/deployment/). The deployment includes the operator and a configuration map. These provide the container image, software, and instructions required to deploy SQL Server instances for the availability group.

* Three nodes, each hosting a [*StatefulSet*](http://kubernetes.io/docs/concepts/workloads/controllers/statefulset/). The StatefulSet contains a pod. Each pod contains:
  * A SQL Server container running one instance of SQL Server.
  * An availability group agent.

* Two [*ConfigMaps*](http://kubernetes.io/docs/tasks/configure-pod-container/configure-pod-configmap/) related to the availability group. The ConfigMaps provide information about:
  * The deployment for the operator.
  * The availability group.

 * Persistent volumes for each instance of SQL Server provide the storage for the data and log files.

In addition, the cluster stores [*secrets*](http://kubernetes.io/docs/concepts/configuration/secret/) for the passwords, certificates, keys, and other sensitive information.

## Compare SQL Server high availabiltiy on containers with and without the availability group

The following table compairs the SQL Server high availability capability in containers on Kubernetes with, and without an availabilty group:

| |With an availability group | Standalone container instance<br/> No availability group
|:------|:------|:------
|Automatically recover from node failure | Yes | Yes
|Automatically recover from pod failure | Yes | Yes
|Faster failover |Yes |
|Automatically recover from SQL Server instance failure | Yes | 
|Automatically recover from database health check failure | Yes | 
|Provide read-only replicas | Yes |
|Secondary replica backup | Yes | 
|Runs as a StatefulSet | Yes | 

One key difference is that the recovery (or failover) time is faster with an availability group than with a single instance of SQL Server in a container. This is because the SQL Server availability group keeps secondary replicas of the availability group databases on other nodes in the cluster. On failover, a secondary replica is selected and promoted to primary. Applications connected to the service are redirected to the new primary replica. 

Without the availability group, when Kubernetes detects a failover, it needs to create the container, connect it to the storage, and then applications connected to the service are reconnected. The exact failover time depends on where the failover was, and how it was detected. 

Generally, the failover time for an availability group is measured in seconds, while the failover time for single instance to recover a container may be up to 10 minutes.

## Next steps

To deploy SQL Server containers in Azure Kubernetes Service (AKS), follow one of these tutorials:

* [Deploy a SQL Server container in Kubernetes](tutorial-sql-server-containers-kubernetes.md).
* [Deploy a SQL Server Always On availability group Kubernetes](tutorial-sql-server-ag-kubernetes.md).

