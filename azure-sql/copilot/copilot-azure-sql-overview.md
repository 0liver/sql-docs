---
title: "Copilot in Azure SQL Database"
description: "Learn more about the features and possibilities of Copilot in Azure SQL Database for administrators and developers."
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.reviewer: josephsack
ms.date: 04/18/2024
ms.service: sql-database
ms.subservice: ai-copilot
ms.topic: overview
monikerRange: "=azuresql||=azuresql-db"
---

# Copilot in Azure SQL Database (preview)

[!INCLUDE [appliesto-sqldb](../includes/appliesto-sqldb.md)]

Copilot in Azure SQL Database is an AI assistant designed to streamline the design, operation, optimization, and health of Azure SQL Database-driven applications. It improves productivity in the Azure portal by offering natural language to SQL conversion and self-help for database administration.

To learn more about the current preview of Copilot in Azure SQL Database, watch the following video:

<br/>

> [!VIDEO https://learn-video.azurefd.net/vod/player?show=data-exposed&ep=introducing-copilot-in-azure-sql-database-private-preview-data-exposed]

> [!NOTE]
> Copilot in Azure SQL Database is currently in a preview for a limited number of early adopters. To sign up for this program, visit [Request Access to Copilot in Azure SQL Database: Preview](https://aka.ms/sqlcopilot-signup).

Copilot provides relevant answers to user questions, simplifying database management by leveraging database context, documentation, dynamic management views, Query Store, and other knowledge sources. For example:

- Database administrators can independently manage databases and resolve issues, or learn more about the performance and capabilities of your database.
- Developers can ask questions about their data as they would in text or conversation to generate a T-SQL query. Developers can also learn to write queries faster through detailed explanations of the generated query.

## Copilot in Azure SQL Database experiences

The preview of Copilot for Azure SQL Database includes two Azure portal experiences:

- [Natural language to SQL](#natural-language-to-sql-query): This experience within the [Azure portal query editor for Azure SQL Database](../database/query-editor.md) translates natural language queries into SQL, making database interactions more intuitive.

- [Azure Copilot integration](#microsoft-copilot-for-azure-enhanced-scenarios): This experience adds Azure SQL Database skills into [Microsoft Copilot for Azure](/azure/copilot/overview), customers with self-guided assistance, empowering them to manage their databases and solve issues independently.

Copilot in Azure SQL Database integrates data and formulate applicable responses using public documentation, dynamic management views, catalog views, and Azure supportability diagnostics.

### Natural language to SQL query

This experience within the [Azure portal query editor for Azure SQL Database](../database/query-editor.md) utilizes table and view names, column names, primary key, and foreign key metadata to generate T-SQL code. You can then review and execute the code suggestion.

This integration means that Copilot can answer questions with prompts like:

 - `Which agents have listed more than two properties for sale?`
 - `Tell me the rank of each agent by property sales and show name, total sales, and rank`
 - Even advanced scenarios such as `Show me a pivot summary table that displays the number of properties sold in each year from 2020 to 2023`

For a tutorial and examples of natural language to SQL capabilities of the Copilot in Azure SQL Database, see [Natural language to SQL in the Azure portal query editor (preview)](query-editor-natural-language-to-sql-copilot.md).

### Microsoft Copilot for Azure enhanced scenarios

You can ask and receive helpful, context-rich suggestions from [Microsoft Copilot for Azure](/azure/copilot/overview) within the Azure portal. 

> [!WARNING]
> Copilot in Azure SQL Database is a preview set of experiences that are powered by large language models (LLMs). Output produced by Copilot might contain inaccuracies, biases, or other unintended content. This occurs because the model powering Copilot in Azure SQL Database was trained on information from the internet and other sources. As with any generative AI model, humans should review the output produced by Copilot before use.

## Sample prompts

Here are a few examples of sample prompts you can provide today for different capability areas:

| Skill Name                     | Skill Description                                                         | Example prompt                                                       |
|------------------------------------------|-----------------------------------------------------------------------------|------------------------------------------------------------------------|
| Active User Connections                 | Shows active user connections to the database.                               | `Who are currently actively connected to the database?`                 |
| Anti-Pattern Query Analysis             | Identifies queries with anti-patterns and their potential impact on performance. | `Show me all the queries in my workload that have anti-patterns in them.` |
| Automatic Tuning Analysis                | Investigates automatic tuning failures and potential solutions.             | `Why is automatic plan correction failing?`                             |
| Basic Database Information              | Retrieves basic information about the database.                            | `What is the name of the logical server for this database?`             |
| Blocking Session Analysis               | Identifies and analyzes blocking sessions.                               | `Check top blocking sessions.`                                          |
| Compatibility Level                      | Provides information about the database compatibility level.               | `What's the compatibility level of this database?`                      |
| Connection String Generation             | Generates the appropriate connection string for the database.            | `Which connection string should I use to connect to my DB?`             |
| Copilot Help                             | Provides general assistance and guidance with Azure SQL.                   | `What can you do related to Azure SQL?`                                |
| Data Synchronization Analysis          | Troubleshoots data synchronization issues, particularly with secondaries.  | `Why do my secondaries not have the latest data?`                     |
| Database and Table Size                 | Provides information about the database and table sizes.                   | `What's the size of this database?`                                     |
| Database Performance Analysis           | Analyzes overall database performance and suggests improvements.        | `Why is my database slow?`                                               | 
| Database Permission Listing             | Lists database permissions and access levels for users.                   | `Which users have access to master database?`                          | 
| Deadlock Analysis                        | Investigates deadlocks and suggests solutions.                           | `Why am I getting deadlock errors? How can I fix it?`                   |
| Dropped Connections Analysis            | Investigates instances of dropped database connections.                   | `Show me all the instances where my database had a dropped connection.` |
| Fragmented Index Analysis               | Identifies fragmented indexes and their impact on performance.         | `Help me find fragmented indexes.`                                      |
| General Anti-Pattern Information        | Provides general information about common SQL anti-patterns.                | `What are the most common SQL anti-patterns?`                           |
| Get Database Names for Server           | Lists all databases on a specific server.                                 | `List all the databases on this server.`                                |
| High CPU Consuming Query Analysis      | Identifies and analyzes queries with high CPU usage.                       | `Why is the CPU usage high on this database?`                          |
| Index Listing                           | Shows all indexes in the database.                                       | `What are all the indexes?`                                            |
| Index Recommendations for Specific Table| Provides index recommendations for one or more tables.                     | `Should I add an index on this table?`                                  |
| Latest Backup Information                | Provides information about the most recent database backup.               | `When was the most recent backup of my database created?`                 |
| MAXDOP Optimization                     | Analyzes and suggests optimizations for the MAXDOP setting.              | `What's the current MAXDOP and how to optimize?`                        |
| Memory Grant Analysis                    | Analyzes memory grant issues and potential causes.                        | `Why am I having memory grant issues?`                                 | 
| Missing Index Suggestions               | Suggests missing indexes to improve query performance.                   | `Missing index suggestion for improving query performance?`            |
| Point-in-Time Restore Retention          | Provides information about the point-in-time restore retention period.  | `How far back in time can I go for a point-in-time restore?`           |
| Query Performance Analysis              | Investigates and suggests solutions for slow-running queries.              | `Why is this query running so slow?`                                   |
| Query Store - Find Forced Plans         | Shows queries with forced plans within a specified timeframe.              | `Show me all the queries from the past 2 days that have forced plans.` |
| Query Store - Find High Execution Time Variation | Identifies queries with high variation in execution time.            | `Which queries on my database have a high variation in execution time?`  |
| Query Store - Find Highest I/O Queries | Shows queries with the highest I/O usage.                                | `What queries on this database use the most I/O?`                      |
| Query Store - Get query text by ID| Shows the query text based on the provided Query ID | `What is the query text for Query ID 1333?`
|
| Query Store - Latest Executed Queries   | Displays the most recently executed queries.                             | `What are the most recently executed queries in my database?`           |
| Query Store - Longest Running Queries   | Shows the longest running queries within a specified timeframe.        | `What are the longest running queries in the past day?`                 | 
| Query Store - Queries with Highest Wait Times | Identifies queries with the highest wait times.                       | `Which queries have had the highest wait times?`                        |
| Query Store - Queries with Multiple Plans | Checks for queries with multiple execution plans.                       | `Show me the queries that have had more than one execution plan.`       |
| Query Store - Regressed Queries         | Identifies queries that have regressed in performance.      | `Have any of my queries gotten significantly slower recently?`          | 
| Query Store - Regressed Queries with Plan Changes | Shows queries with plan changes that have regressed in performance.       | `Are there any queries that had plan changes and regressed in performance?`|
| Query Store - Show Executions per Query | Displays the number of executions for each query.                        | `What queries are being executed most often?`                          |
| Query Store - Top Resource Consuming Queries | Identifies and analyzes queries with the highest resource consumption.   | `What are the most expensive queries in my workload?`                  |
| Query Store Mode Troubleshooting        | Investigates and provides solutions for Query Store being in read-only mode.| `Why is Query Store in read-only mode? How can I fix it?`                |
| Related Documentation                   | Provides links to relevant documentation based on the user's query.       | `What does database compatibility level mean?`                         |
| Resource Usage Analysis                  | Analyzes resource usage and potential bottlenecks.                        | `Is the database hitting resource limits? Which limits?`               |
| Table Listing                           | Lists all tables in the database.                                        | `What are the names of all the tables?`                                 | 
| Wait Statistics Analysis                 | Analyzes wait statistics and potential performance bottlenecks.         | `What do the wait statistics look like for my database?`                 | 
| Workload Increase and Scaling Analysis  | Assesses workload increases and potential need for scaling.                 | `Has increased workload or traffic caused performance issues?`         |

## Examples

- When you're working with a slow Azure SQL Database, you could provide the prompt `My database is slow`.

   Microsoft Copilot for Azure (preview) starts looking at your database based on your context in the Azure portal. After the check, Copilot will detail specific areas that might be contributing to the issue. In this example, there was a specific query driving high CPU utilization:

   :::image type="content" source="media/copilot-azure-sql-overview/slow-database.png" alt-text="Screenshot showing the query prompt and generated sample query about high CPU utilization." lightbox="media/copilot-azure-sql-overview/slow-database.png":::

- You can continue the conversation and investigation with a prompt of `How can I tune that high CPU query?`.

   Copilot understands that this prompt refers to the query identified earlier, and provides a new index suggestion:

   :::image type="content" source="media/copilot-azure-sql-overview/high-cpu-query.png" alt-text="Screenshot showing a second query prompt in the conversation and generated sample query about high CPU utilization." lightbox="media/copilot-azure-sql-overview/high-cpu-query.png":::

## Responsible AI

For more information on how Microsoft implements responsible AI tools in [Microsoft Copilot for Azure](/azure/copilot/overview), see [Responsible AI FAQ for Microsoft Copilot for Azure (preview)](/azure/copilot/responsible-ai-faq).

## Related content

- [Frequently asked questions about Copilot in Azure SQL Database (preview)](copilot-azure-sql-faq.yml)
- [Natural language to SQL in the Azure portal query editor (preview)](query-editor-natural-language-to-sql-copilot.md)
- [Intelligent applications with Azure SQL Database](../database/ai-artificial-intelligence-intelligent-applications.md)
