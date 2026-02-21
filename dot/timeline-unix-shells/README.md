# Unix Shell Timeline

## Overview
This example demonstrates how to use **iterative SQL queries** in Relationship Visualizer to generate a timeline from a list of values using SQL‑driven logic.

| ![](./Graph%20-%20All%20Styles.png) |
| :---------------------------------: |

The example uses a worksheet named **shells**, containing Unix shell names and the years they were introduced. The goal is to place all shells from the same year on the same rank and then build a timeline showing how shells evolved over time.

## Scenario
You want to build a timeline graph showing the evolution of Unix shells. The workbook includes a worksheet named `shells` with two fields: the year a shell was introduced and the shell’s name. Because multiple shells may share the same year, the goal is to group all shells from that year into a single rank in the final visualization.

## Data Source
The worksheet **shells** contains the following fields:

- **Year** — the year a shell was introduced
- **Shell** — the name of the Unix shell

Each row represents one shell. The data appears as:

| <b>Year</b> | <b>Shell</b> |
| ----------- | ------------ |
| future      | ksh-POSIX    |
| future      | POSIX        |
| 1972        | Thompson     |
| 1976        | Bourne       |
| 1976        | Mashey       |
| 1978        | Formshell    |
| 1978        | csh          |
| 1980        | esh          |
| 1980        | vsh          |
| 1982        | ksh          |
| 1982        | System-V     |
| 1984        | v9sh         |
| 1984        | tcsh         |
| 1986        | ksh-i        |
| 1988        | rc           |
| 1988        | KornShell    |
| 1988        | Perl         |
| 1990        | Bash         |
| 1990        | tcl          |

## Goal of the Example
- Group all shells from the same year into a shared rank  
- Iterate through the list of unique years without hard‑coding  
- Build a timeline by connecting years in ascending order  
- Add evolution edges showing how shells influenced one another  
- Produce a clean, readable diagram combining ranks, timelines, and relationships  

## Step 1 — Create a subgroup for a single year
Relationship Visualizer includes a SQL extension that lets you define subgroups directly within a query. This is useful when you want multiple items to share the same position or rank in the final diagram. In this scenario, each year may have several shells associated with it, and we want all shells from the same year to appear together.

You can create these subgroups using the `CREATE RANK` clause, shown below:
``` sql
TRUE AS [CREATE RANK]
```

To group all shells from 1988 into a single rank in the final diagram, the query is written as follows:

```sql
SELECT [Shell] AS [ITEM],
       TRUE    AS [CREATE RANK],
       'same'  AS [RANK]
FROM  [shells$]
WHERE [Year] = '1988'
```

This produces:

``` dot
{ rank="same"; "1988"; "KornShell"; "Perl"; "rc"; }
```

## Step 2 — Retrieve the list of unique years
The previous query handles a single year, but we want to repeat the process for every year in the dataset without hard‑coding values. The next step is to retrieve the full list of years, removing duplicates with a standard `SELECT DISTINCT` query:

```sql
SELECT DISTINCT [Year] AS [ID] FROM [shells$]
```

resulting in:


| <b>ID</b> |
| ----------- |
| future      |
| 1972        |
| 1976        |
| 1978        |
| 1980        |
| 1982        |
| 1984        |
| 1986        |
| 1988        |
| 1990        |

This list becomes the driver for iteration.

## Step 3 — Iterate the year list
This step combines Step 2 with Step 1. For this step, we supply an additional `ITERATE` SQL extension flag.

``` SQL
TRUE AS [ITERATE]
```

This flag tells Relationship Visualizer that the query is iterative. It causes the engine to run two SQL statements: one to generate the list of IDs, and another that executes once for each ID in that list.

Two SQL statements are supplied:

- `[SQL FOR ID]` — produces the list of IDs  
- `[SQL FOR DATA]` — runs once per ID  

```sql
SELECT
  'SELECT DISTINCT [Year] AS [ID] FROM [shells$]' AS [SQL FOR ID],
  'SELECT [Shell] AS [ITEM], TRUE AS [CREATE RANK], ''same'' AS [RANK]
     FROM [shells$] WHERE [Year] = ''{ID}''' AS [SQL FOR DATA],
  TRUE AS [ITERATE]
```

Each `{ID}` is substituted and executed, producing one subgroup per year.

> [!TIP]
> Because the SQL statements are passed as strings, any string values inside them—such as `'{ID}'`—must be escaped using doubled single quotes (`''{ID}''`).

Relationship Visualizer loops through the ID list produced by the first query. Each ID is substituted into the second query wherever `{ID}` appears, and the resulting query is executed. The generated rows are written to the `data` worksheet.

Using the sample data above, this pair of SQL statements adds the following entries to the `data` worksheet:

``` dot
{ rank="same"; "Thompson"; }
{ rank="same"; "Bourne"; "Mashey"; }
{ rank="same"; "csh"; "Formshell"; }
{ rank="same"; "esh"; "vsh"; }
{ rank="same"; "ksh"; "System-V"; }
{ rank="same"; "tcsh"; "v9sh"; }
{ rank="same"; "ksh-i"; }
{ rank="same"; "KornShell"; "Perl"; "rc"; }
{ rank="same"; "Bash"; "tcl"; }
{ rank="same"; "ksh-POSIX"; "POSIX"; }
```

## Step 4 — Add the year itself to each subgroup
The subgroups based on year have been created, but notice that the year itself is not included. How do we know which shells belong to which years?

In SQL, the `UNION` operator functions like an “AND” that allows you to run additional queries and return the unique combined results.

The following statement can supply the year:

``` sql
SELECT [Year] AS [ITEM] WHERE [YEAR] = '{ID}'
```

`UNION` has a restriction that all SQL statements must return the same list of fields. To comply with this rule, the statement becomes:

``` sql
SELECT [Year]  AS [ITEM], TRUE AS [CREATE RANK], 'same' AS [RANK] FROM [shells$] WHERE [YEAR] = '{ID}'
```

The `UNION` is added to the statement as follows:

``` sql
SELECT 
  'SELECT DISTINCT [Year] AS [ID] FROM [shells$]' AS [SQL FOR ID],
  'SELECT [Year]  AS [ITEM], TRUE AS [CREATE RANK], ''same'' AS [RANK] FROM [shells$] WHERE [YEAR] = ''{ID}'' 
      UNION
   SELECT [Shell] AS [ITEM], TRUE AS [CREATE RANK], ''same'' AS [RANK] FROM [shells$] WHERE [YEAR] = ''{ID}'' ' AS [SQL FOR DATA],
  TRUE AS [ITERATE]
```

When the query is run, the following subgroups are produced:

``` dot
{ rank="same"; "1972"; "Thompson"; }
{ rank="same"; "1976"; "Bourne"; "Mashey"; }
{ rank="same"; "1978"; "csh"; "Formshell"; }
{ rank="same"; "1980"; "esh"; "vsh"; }
{ rank="same"; "1982"; "ksh"; "System-V"; }
{ rank="same"; "1984"; "tcsh"; "v9sh"; }
{ rank="same"; "1986"; "ksh-i"; }
{ rank="same"; "1988"; "KornShell"; "Perl"; "rc"; }
{ rank="same"; "1990"; "Bash"; "tcl"; }
{ rank="same"; "future"; "ksh-POSIX"; "POSIX"; }
```

Now the subgroups contain both the year and the shells introduced during that year.

## Step 5 — Build a timeline of years
Next, we want to build a timeline. For this, we use another Relationship Visualizer SQL extension.

Similar to the `CREATE RANK` keyword, there is a `CREATE EDGES` keyword, specified as:

``` sql
TRUE AS [CREATE EDGES]
```

When this query runs, it generates a set of rows in the `data` worksheet that create an edge from each item to the next. If the data contains `a`, `b`, `c`, the resulting edges will be `a` → `b` → `c`.

The single statement:

``` sql
SELECT DISTINCT [Year] AS [ITEM], 
       TRUE            AS [CREATE EDGES] 
FROM   [Shells$] 
WHERE  [Year] IS NOT NULL 
ORDER BY [Year] ASC
```

produces the following chain of edges from year to year.

| ![](./timeline.png) |
| --------------------------------- |

## Step 6 — Combine the timeline with the subgroups
By merging the iterative subgroup logic with the timeline query, the diagram now shows:

- A horizontal timeline of years  
- A vertical stack of shells for each year  
- All shells from the same year aligned on the same rank  

The resulting graph becomes:

| ![](./timeline_and_subgraphs.png) |
| --------------------------------- |

## Step 7 — Add evolution edges
Our data also includes a worksheet named `evolution`, which tells us which shells preceded other shells. The data looks as follows:

| <b>From Shell</b> | <b>To Shell</b> |
| ----------------- | --------------- |
| Thompson          | Bourne          |
| Thompson          | Mashey          |
| Thompson          | csh             |
| Bourne            | v9sh            |
| Bourne            | ksh             |
| Bourne            | esh             |
| Bourne            | vsh             |
| Bourne            | Formshell       |
| Bourne            | Bash            |
| Bourne            | System-V        |
| Formshell         | ksh             |
| csh               | ksh             |
| ksh               | ksh-i           |
| csh               | tcsh            |
| System-V          | POSIX           |
| v9sh              | rc              |
| KornShell         | ksh-POSIX       |
| KornShell         | POSIX           |
| KornShell         | Bash            |
| esh               | ksh             |
| vsh               | ksh             |
| ksh-i             | KornShell       |
| ksh-i             | Bash            |

It is a simple change to add the following SQL statement:

``` sql
SELECT [From Shell] AS [ITEM], 
       [To Shell]   AS [RELATED ITEM] 
FROM   [evolution$]

```

which changes the graph to:

| ![](./shell_evolution.png) |
| -------------------------- |

## Step 8 — Apply styles and adjust layout
The last step is to create styles and apply them to the nodes and edges. 
The fully styled graph becomes:


| ![](./Graph%20-%20All%20Styles.png) |
| ------------------------- |

## Summary
This example demonstrates how to:

- Use `ITERATE` SQL to generate repeated subgraphs  
- Group items by year using `CREATE RANK`  
- Build a timeline using `CREATE EDGES`  
- Combine multiple SQL extensions into a single coherent diagram  
- Add evolution relationships to show how Unix shells developed over time  

The result is a clear, structured visualization that blends ranks, timelines, and dependency edges using a minimal, repeatable SQL pattern.