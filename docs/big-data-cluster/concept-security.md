---
title: Security concepts for SQL Server Big Data Cluster | Microsoft Docs
description:
author: nelgson 
ms.author: ms.author:negust 
manager: craigg
ms.date: 08/06/2018
ms.topic: conceptual
ms.prod: sql
---

# Security concepts for SQL Server Big Data Cluster

A secure Big Data cluster implies consistent and coherent support for authentication and authorization scenarios, across both SQL Server and HDFS/Spark. Authentication is the process of verifying the identity of a user or service and ensuring they are who they are claiming to be. Authorization refers to granting or denying of access to specific resources based on the requesting user's identity. This step is performed after a user is identified through authentication.

Authorization in Big Data context is usually performed through access control lists (ACLs), which associate user identities with specific permissions. HDFS supports authorization by limiting access to service APIs, HDFS files and job execution.

This topic will cover the key security related concepts in the Big Data cluster.

## Cluster Endpoints

There are three entry points to the Big Data Cluster

* HDFS/Spark (Knox) gateway – This is an HTTPS-based endpoint. Other endpoints are proxied through this. HDFS/Spark gateway is used for accessing services like webHDFS and Livy. Wherever you see references to Knox, this is the endpoint.

* Controller endpoint – Big Data Cluster  management service that exposes REST APIs for managing the cluster. Some tools like the Admin portal is also accessed through this endpoint.

* Master Instance  - TDS endpoint for database tools and applications to connect to SQL Server Master Instance in the cluster.

Currently, there is no option of opening up additional ports for accessing the cluster from the outside.

### How endpoints are secured

Securing endpoints in the Big Data Cluster is done using passwords that can be set/updated either using environment variables or CLI commands. All cluster internal passwords are stored as Kubernetes secrets.  

# Authentication

Upon provisioning the cluster, a number of logins are created.
Some of this logins are for services to communicate with each other, and others are for end-users to access the cluster.

## End-user authentication
Upon provisioning the cluster, a set of end-user passwords need to be set using environment variables. These are passwords that SQL administrators and cluster administrators use to access services:

Controller username:
 + CONTROLLER_USERNAME=<controller_username>

Controller password:  
 + CONTROLLER_PASSWORD=<controller_password>

SQL Master SA password: 
 + MSSQL_SA_PASSWORD=<controller_sa_password>

Username for accessing the HDFS/Spark endpoint:
 + KNOX_USERNAME=<know_username> 

Password for accessing the HDFS/Spark endpoint:
 + KNOX_PASSWORD=<knox_password>
 
### Update HDFS/Spark (Knox) and Controller passwords
In order to update Knox and controller admin passwords, the following command can be used:

   ```bash
    mssqlctl set-password service [-h] [-v] servicename clustername
   ```

  Service name indicated which service to update the password for. This can be either "knox" or "controller"


Optional arguments:
 *  -h, --help     show this help message and exit
 * -v, --verbose  Increase output verbosity

For example, to update the Knox password for a cluster called "testcluster", run the following:

  ```bash
export KNOX_PASSWORD = <password>
mssqlctl set-password service knox testcluster
   ```

It is currently not possible to change the Knox user name.

## Intra cluster authentication

 Upon deployment of the cluster, a number of SQL logins are created:

* A special SQL login is created in the Controller SQL instance that is system managed, with sysadmin role. The password for this login is captured as a K8s secret.

* A sysadmin login is created in all SQL instances in the cluster, that Controller owns and manages. It is required for Controller to perform administrative tasks on these instances (i.e. HA setup, upgrade etc.). These logins are also used for intra cluster communication between SQL instances, i.e. Master instance communicating with Data Pool.

Please note that in CTP2.0, only basic authentication is supported. Fine-grained access control to HDFS objects, and SQL Big Data Cluster compute and data pools, is not yet available.

## Intra cluster communication

Communication with non-SQL services within the Big Data Cluster, like for example Livy to Spark or Spark to Storage Pool, is secured using certificates. All SQL Sever to SQL Server communication is secured using SQL logins.

