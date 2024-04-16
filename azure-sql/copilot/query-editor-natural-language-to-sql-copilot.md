---
title: Natural language to SQL in the Azure portal query editor
titleSuffix: Azure SQL Database
description: Learn how to write natural language prompts to generate T-SQL queries using Copilot for Azure SQL Database in the Azure portal query editor.
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.reviewer: josephsack
ms.date: 04/16/2024
ms.service: sql-database
ms.subservice: ai-copilot
ms.topic: how-to
monikerRange: "=azuresql||=azuresql-db"
---
# Natural language to SQL in the Azure portal query editor (preview)
[!INCLUDE [appliesto-sqldb](../includes/appliesto-sqldb.md)]

Part of the [Copilot in Azure SQL Database (preview)](copilot-azure-sql-overview.md), the [Natural language to SQL](copilot-azure-sql-overview.md?view=azuresql-db&preserve-view=true#natural-language-to-sql-query) experience within the [Azure portal query editor](../database/query-editor.md) translates natural language queries into SQL, making database interactions more intuitive.

In this article, learn how to write natural language prompts to generate T-SQL queries using Copilot for Azure SQL Database in the Azure portal query editor.

> [!WARNING]
> Copilot in Azure SQL Database is a preview set of experiences that are powered by large language models (LLMs). Output produced by Copilot might contain inaccuracies, biases, or other unintended content. This occurs because the model powering Copilot was trained on information from the internet and other sources. As with any generative AI model, humans should review the output produced by Copilot before use.

## Natural language to SQL query

Copilot in Azure SQL Database utilizes table and view names, column names, primary key, and foreign key metadata to generate T-SQL code. You then review and execute the code suggestion. 

> [!NOTE]
> Copilot in Azure SQL Database does not use data values to generate Transact-SQL suggestions.

This integration means that Copilot can answer questions with prompts like:

- `Which agents have listed more than two properties for sale?`
- `Tell me the rank of each agent by property sales and show name, total sales, and rank`
- Even advanced scenarios such as `Show me a pivot summary table that displays the number of properties sold in each year from 2020 to 2023`

## Prerequisites

- An existing Azure account and Azure SQL database.
- Acceptance into the preview. To sign up for this program, visit [Request Access to Copilot in Azure SQL Database: Preview](https://aka.ms/sqlcopilot-signup).

> [!IMPORTANT]
> Review these [preview terms](https://azure.microsoft.com/support/legal/preview-supplemental-terms/#AzureOpenAI-PoweredPreviews) before using natural language to SQL in Copilot in Azure SQL Database.

## Generate a query

As a preview feature, once enrolled, you can find Copilot in Azure SQL Database integrated with the query editor.

To use natural language to SQL in Copilot in Azure SQL Database, follow these steps:

1. In the query editor toolbar, select the **Launch inline copilot** button.
1. In the **Table selection** drop-down menu, select the table and views for consideration by Copilot.
1. Type your question in the input box. When ready, select the **Generate Query** button. This will generate the suggested T-SQL code statement.
1. Click the **Accept** or **Decline** button for the generated T-SQL query.
1. Execute the generated T-SQL query by selecting **Run**, or you can edit the prompt and regenerate new code. This regeneration will append, and not replace, any existing code in your query editor window.

> [!NOTE]
> This feature currently only supports the generation of SELECT statements.

## Give feedback

We use feedback on generated queries to help improve Copilot in Azure SQL Database. This feedback is crucial to improving the quality of the suggestions.

1. To send feedback on queries, click the thumbs-up or thumbs-down beneath the **Generate Query** button.
1. Within the feedback dialog, provide the nature of the feedback, a description of what went right or wrong, and then an option to share your prompts with Microsoft.
1. Select **Submit**.

## Write effective prompts

Here are some tips for writing effective prompts.

- When crafting prompts, be sure to start with a clear and concise description of the specific information you're looking for.

- Natural language to SQL depends on expressive table and column names.  If your table and columns aren't expressive and descriptive, Copilot might not be able to construct a meaningful query.

- Use natural language that is applicable to your table and view names, column names, primary keys, and foreign keys of your database. This context helps Copilot generate accurate queries. Specify what columns you wish to see, aggregations, and any filtering criteria as explicitly as possible. Copilot should be able to correct typos or understand context given your schema context.

- To avoid ambiguity or incorrect table selections, consider filtering down the specific tables of interest in the table selection dropdown list.

- Avoid ambiguous or overly complex language in your prompts. Simplify the question while maintaining its clarity. This editing ensures Copilot can effectively translate it into a meaningful T-SQL query that retrieves the desired data from the associated tables and views.

- Currently, natural language to SQL supports English language to T-SQL.

- The following example prompts are clear, specific, and tailored to the properties of your schema and database, making it easier for Copilot to generate accurate Transact-SQL queries:
  - `Show me all properties that sold last year`
  - `Count all the products, group by each category`
  - `Show all agents who sell properties in California`
  - `Show agents who have listed more than two properties for sale`
  - `Show the rank of each agent by property sales and show name, total sales, and rank`
  - `Show me a pivot summary table that displays the number of properties sold in each year from 2020 to 2023`

## Related content

- [Copilot in Azure SQL Database (preview)](copilot-azure-sql-overview.md)
- [Frequently asked questions about Copilot in Microsoft Azure SQL (preview)](copilot-azure-sql-faq.yml)
- [Azure portal query editor for Azure SQL Database](../database/query-editor.md)
