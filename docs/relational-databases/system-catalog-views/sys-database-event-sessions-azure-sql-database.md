---
description: "sys.database_event_sessions (Azure SQL Database)"
title: "sys.database_event_sessions"
titleSuffix: Azure SQL Database
ms.custom: ""
ms.date: "03/30/2022"
ms.service: sql-database
ms.reviewer: ""
ms.topic: "reference"
dev_langs: 
  - "TSQL"
ms.assetid: 02c2cd71-d35e-4d4c-b844-92b240f768f4
author: rwestMSFT
ms.author: randolphwest
monikerRange: "= azuresqldb-current"
---
# sys.database_event_sessions (Azure SQL Database)
[!INCLUDE[Azure SQL Database](../../includes/applies-to-version/asdb.md)]

Lists all the event session definitions that exist in the current database.  
  
> [!NOTE]
>  The similar catalog view named `sys.server_event_sessions` applies only to [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
|Column name|Data type|Description|  
|-----------------|---------------|-----------------|  
|event_session_id|**int**|The unique ID of the event session. Is not nullable.|  
|name|**sysname**|The user-defined name for identifying the event session. name is unique. Is not nullable.|  
|event_retention_mode|**nchar(1)**|Determines how event loss is handled. The default is S. Is not nullable. Is one of the following:<br /><br /> S. Maps to event_retention_mode_desc = ALLOW_SINGLE_EVENT_LOSS<br /><br /> M. Maps to event_retention_mode_desc = ALLOW_MULTIPLE_EVENT_LOSS<br /><br /> N. Maps to event_retention_mode_desc = NO_EVENT_LOSS|  
|event_retention_mode_desc|**sysname**|Describes how event loss is handled. The default is ALLOW_SINGLE_EVENT_LOSS. Is not nullable. Is one of the following:<br /><br /> ALLOW_SINGLE_EVENT_LOSS. Events can be lost from the session. Single events are dropped only when all event buffers are full. Losing single events when buffers are full allows for acceptable [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] performance characteristics, while minimizing the loss in the processed event stream.<br /><br /> ALLOW_MULTIPLE_EVENT_LOSS. Full event buffers can be lost from the session. The number of events lost depends on the memory size allocated to the session, the partitioning of the memory, and the size of the events in the buffer. This option minimizes performance impact on the server when event buffers are quickly filled. However, large numbers of events can be lost from the session.<br /><br /> NO_EVENT_LOSS. No event loss is allowed. This option ensures that all events raised are retained. Using this option forces all the tasks that fire events to wait until space is available in an event buffer. This may lead to detectable performance degradation while the event session is active.|  
|max_dispatch_latency|**int**|The amount of time, in milliseconds, that events will be buffered in memory before they are served to session targets. Valid values are from 1 to 2147483648, and -1. A value of -1 indicates that dispatch latency is infinite. Is nullable.|  
|max_memory|**int**|The amount of memory allocated to the session for event buffering. The default value is 4 MB. Is nullable.|  
|max_event_size|**int**|The amount of memory set aside for events that do not fit in event session buffers. If max_event_size exceeds the calculated buffer size, two additional buffers of max_event_size are allocated to the event session. Is nullable.|  
|memory_partition_mode|**nchar(1)**|The location in memory where event buffers are created. The default partition mode is G. Is not nullable. memory_partition_mode is one of:<br /><br /> G - NONE<br /><br /> C - PER_CPU<br /><br /> N - PER_NODE|  
|memory_partition_mode_desc|**sysname**|The default is NONE. Is not nullable. Is one of the following:<br /><br /> NONE. A single set of buffers are created within a SQL Server instance.<br /><br /> PER_CPU. A set of buffers is created for each CPU.<br /><br /> PER_NODE. A set of buffers is created for each non-uniform memory access (NUMA) node.|  
|track_causality|**bit**|Enable or disable causality tracking. If set to 1 (ON), tracking is enabled and related events on different server connections can be correlated. The default setting is 0 (OFF). Is not nullable.|  
|startup_state|**bit**|Value determines whether or not session is started automatically when the server starts. The default is 0. Is not nullable. Is one of:<br /><br /> 0 (OFF). The session does not start when the server starts.<br /><br /> 1 (ON). The event session starts when the server starts.|  
  
## Permissions  

Requires the VIEW SERVER STATE permission.  

## Next steps

Learn more about related concepts in the following articles:

- [Monitoring Microsoft Azure SQL Database and Azure SQL Managed Instance performance using dynamic management views](/azure/azure-sql/database/monitoring-with-dmvs)
- [Extended events in Azure SQL Database](/azure/azure-sql/database/xevent-db-diff-from-svr)
- [sys.database_event_session_actions (Azure SQL Database)](sys-database-event-session-actions-azure-sql-database.md)
- [sys.database_event_session_targets (Azure SQL Database)](sys-database-event-session-targets-azure-sql-database.md)
- [sys.database_event_session_events (Azure SQL Database)](sys-database-event-session-events-azure-sql-database.md)