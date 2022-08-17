---
title: "Graph Functions (Transact-SQL)"
description: "Graph Functions (Transact-SQL)"
author: "arvindshmicrosoft"
ms.author: "arvindsh"
ms.date: 08/16/2022
ms.prod: sql
ms.technology: t-sql
ms.topic: reference
helpviewer_keywords:
  - "SQL Graph functions"
dev_langs:
  - "TSQL"
monikerRange: "= azuresqldb-current || >= sql-server-2017 || >= sql-server-linux-2017 || = azuresqldb-mi-current"
---
# Graph Functions (Transact-SQL)
[!INCLUDE [SQL Server Azure SQL Database Azure SQL Managed Instance](../../includes/applies-to-version/sqlserver2017-asdb-asdbmi.md)]

Use the functions described on the pages in this section to extract values from, and transform values to, the pseudo-columns used in SQL Graph.
  
|Function|Description|  
|--------------|-----------------|  
| [OBJECT_ID_FROM_NODE_ID](../../t-sql/functions/object-id-from-node-id-transact-sql.md)	|Extract the `object_id` from a `node_id`  |
| [GRAPH_ID_FROM_NODE_ID](../../t-sql/functions/graph-id-from-node-id-transact-sql.md)	|Extract the `graph_id` from a `node_id`  |
| [NODE_ID_FROM_PARTS](../../t-sql/functions/node-id-from-parts-transact-sql.md)	|Construct a node_id from an `object_id` and a `graph_id`  |
| [OBJECT_ID_FROM_EDGE_ID](../../t-sql/functions/object-id-from-edge-id-transact-sql.md)	|Extract `object_id` from `edge_id`  |
| [GRAPH_ID_FROM_EDGE_ID](../../t-sql/functions/graph-id-from-edge-id-transact-sql.md)	|Extract the `graph_id` for a given `edge_id`  |
| [EDGE_ID_FROM_PARTS](../../t-sql/functions/node-id-from-parts-transact-sql.md)	|Construct `edge_id` from `object_id` and `graph_id`  |
 
## See Also

- [SQL Graph Architecture](../../relational-databases/graphs/sql-graph-architecture.md)  
- [SQL Graph Database Sample](../../relational-databases/graphs/sql-graph-sample.md)
- [OPENROWSET Bulk Rowset Provider](../../relational-databases/import-export/-bulk-import-large-object-data-with-openrowset-bulk-rowset-provider.md)
- [GRAPH_ID_FROM_EDGE_ID](./graph-id-from-edge-id-transact-sql.md)
- [GRAPH_ID_FROM_NODE_ID](./graph-id-from-node-id-transact-sql.md)
 