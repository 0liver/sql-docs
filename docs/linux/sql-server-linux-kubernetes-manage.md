---
title: SQL Server Always On availability group Kubernetes specification
description: This article explains the parameters for the SQL Server Kubernetes Always On availability group specification
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.date: 08/09/2018
ms.topic: article
ms.prod: sql
ms.component: ""
ms.suite: "sql"
ms.custom: "sql-linux"
ms.technology: linux
monikerRange: ">=sql-server-ver15||>=sql-server-linux-ver15||=sqlallproducts-allversions"
---
# SQL Server Always On Availability Group - Kubernetes specification

To configure an Always On Availability Group on Kubernetes, create a specification. The specification is a `.yaml` file.  

Shows basic management tasks for Kubernetes. The examples in this article apply to all Kubernetes cluster. The scenarios in these examples are applied against a cluster on Azure Kubernetes Service.

See an example of the of the end-to-end deployment in [this tutorial](tutorial-sql-server-ag-kubernetes.md).

## Create a manifest file for the specification

The following example of a manifest file describes a Kubernetes specification for SQL Server. Copy the contents of the example into a new file named `sqlserver.yaml` to create the SQL Server availability group StatefulSet in Kubernetes.

[sqlserver.yaml](https://github.com/Microsoft/sql-server-samples/tree/master/samples/features/high%20availability/Linux)

To deploy the SQL Server instances and create the availability group, run the following command.

```azurecli
kubectl apply -f sqlserver.yaml
```

## Fail over - SQL Server availability group on Kubernetes

To fail over an Always On availability group primary replica to a different node in Kubernetes, use a job. This article identifies the environment variables for this job.

The following example of a manifest file describes a job to manually fail over job for an availability group on a Kubernetes replica. Copy the contents of the example into a new file called `ag-failover.yaml`.

[ag-failover.yaml](https://github.com/Microsoft/sql-server-samples/tree/master/samples/features/high%20availability/Linux)

```azurecli
kubectl apply -f ag-failover.yaml
```

When you apply manifest file, Kubernetes runs the job. When the job runs, the supervisor elects a new leader and moves the primary replica to the SQL Server instance of the leader.

After you run the job, delete it. The job object in Kubernetes remains after completion so you can view its status. You have to manually delete old jobs after noting their status. Deleting the job also deletes the Kubernetes logs. If you do not delete the job, future failover jobs will fail unless you change the job name and the pod selector. For more information, see [Jobs - Run to Completion](https://kubernetes.io/docs/concepts/workloads/controllers/jobs-run-to-completion/).

## Rotate credentials

[rotate-creds.yaml](https://github.com/Microsoft/sql-server-samples/tree/master/samples/features/high%20availability/Linux)

```azurecli
kubectl apply -f ag-failover.yaml
```

## Next steps

[SQL Server availability group on Kubernetes cluster](sql-server-ag-kubernetes.md)