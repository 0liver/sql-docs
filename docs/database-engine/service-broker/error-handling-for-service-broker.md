﻿---
title: Error Handling for Service Broker
description: "Error handling in an application that uses Service Broker has two distinct aspects."
ms.prod: sql
ms.technology: configuration
ms.topic: conceptual
author: markingmyname
ms.author: maghan
ms.reviewer: mikeray
ms.date: "03/30/2022"
---

# Error Handling for Service Broker

[!INCLUDE [sql-asdbmi](../../includes/applies-to-version/sql-asdbmi.md)]

Error handling in an application that uses Service Broker has two distinct aspects. First, the application must handle errors raised by the Database Engine through the normal Transact-SQL error mechanism. Second, an application that uses Service Broker must handle asynchronous errors that arrive as messages on the queue for the service. In either case, an application must not permanently remove a message from a queue without acting on the message, and the application must always hold a conversation group lock before updating state that is related to the conversation group.

## In This Section
  - [Handling Transact-SQL Errors (Service Broker)](handling-transact-sql-errors.md)  
    Describes strategies for working with Transact-SQL errors while maintaining transactional messaging.

  - [Handling Service Broker Error Messages](handling-service-broker-error-messages.md)  
    Describes strategies for handling error messages delivered by Service Broker.

  - [Handling Poison Messages](handling-poison-messages.md)  
    Describes strategies for recovering from poison messages.

## See also[Understanding Database Engine Errors](../../relational-databases/errors-events/understanding-database-engine-errors.md)

