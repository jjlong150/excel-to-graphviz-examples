# History Range Timelines

## Overview

This package demonstrates how to generate **history range timelines** using the Relationship Visualizer with SQL‑driven queries. 

| ![](./Graph%20-%20All%20Styles.png) |
| :---------------------------------: |

The included workbook, `musicians.xlsx`, provides a curated dataset of musicians and bands with active years, genres, and descriptive metadata suitable for timeline visualization.

The SQL statements transform these flat records into a **genre‑aligned historical timeline**, showing when artists were active and how musical styles evolved across decades.

The sample includes a large number of records to demonstrate that the timeline can handle substantial datasets without difficulty.

## Quick Start

To generate a history timeline:

1. Open the **Relationship Visualizer** workbook.  
2. Navigate to the **SQL** worksheet.  
3. Ensure the **DATA FILE** points to `musicians.xlsx`.  
4. Click **Run SQL Commands**.  
5. The timeline will appear on the **graph** worksheet.  
6. Use the **Graphviz ribbon tab** to adjust layout, spacing, and visual settings.

This is the fastest way to explore the historical range of the dataset.

## Workbook Structure

### musicians.xlsx
Only a subset of fields in the workbook is used by the SQL pipeline. These fields drive the timeline:

| Field | Purpose |
|-------|---------|
| **Band** | Display name used in the timeline. |
| **Genre** | Category used for node and edge styling. |
| **Band Type** | Additional descriptor included in tooltips. |
| **Origin** | Country or region of origin (tooltip only). |
| **Years Active** | Descriptive text included in tooltips. |
| **Year From** | Start year for the timeline edge. |
| **Year To** | End year for the timeline edge. |
| **Description** | Long‑form tooltip text. |

These fields are sufficient to construct a continuous timeline, style nodes by genre, and provide meaningful hover‑tooltips.

## SQL Queries for Relationship Visualizer

The SQL pipeline in `sql.txt` builds a complete **genre‑aligned historical timeline**.  
The queries:

- Load genre definitions  
- Create a continuous year backbone  
- Style year nodes  
- Draw start‑to‑finish edges for each band  
- Group items by year using `ITERATE`  
- Apply genre‑based styling to nodes and edges  
- Add tooltips containing descriptive metadata  

These queries can be extended to support additional genres, eras, or metadata.

## Techniques Used to Create the Timeline

This timeline demonstration uses the following techniques:

- Standard SQL queries to create nodes and edges  
- Placeholder substitution for dynamic values  
- `ENUMERATE` to generate continuous year sequences  
- `ITERATE` to group bands by year  
- Genre‑based node and edge styling  
- Tooltips containing descriptive metadata  
- Cluster boundaries to provide a titled canvas  

The result is a clean, navigable timeline showing how musical activity spans decades and genres.

## How the SQL Builds the History Timeline

The Relationship Visualizer constructs the timeline using a structured sequence of operations:

```
    [Step 1] Create genre legend
            │
            ▼
    [Step 2] Open the border around the graph
            │
            ▼
    [Step 3] Create continuous year backbone
            │
            ▼
    [Step 4] Style year nodes
            │
            ▼
    [Step 5] Draw start–finish edges for each band
            │
            ▼
    [Step 6] Group items by year using ITERATE
            │
            ▼
    [Step 7] Apply genre‑based node styling
            │
            ▼
    [Step 8] Close the border around the graph
            │
            ▼
    Final Graph (Preview / Publish)
```

## Steps to Build the History Timeline

### Step 1 - Create Genre Legend

Create a **genre legend** by reading all genre definitions and styling them as nodes. Transparent edges ensure the legend appears at the bottom of the graph.

```sql
SELECT TRUE               AS [CREATE EDGES],
       [Genre]            AS [Item], 
       'edge_transparent' AS [Style Name]       
FROM   [genre$]
ORDER BY [genre] ASC

SELECT [Genre]                                     AS [Item], 
       '""'                                        AS [Label],
       [Genre]                                     AS [External Label],
       [Genre] & ' | ' & [Definition]              AS [Tooltip],
       'node_genre_' & REPLACE([Genre], ' ', '_' ) AS [Style Name] 
FROM   [genre$] 
ORDER BY [genre] ASC
```

### Step 2 - Open the border around the graph

Place all timeline entries in one giant cluster which lets us assign a heading and canvas tooltip. The heading is dynamic, finding the range of years in the dataset.

``` sql
SELECT '{'                                                                                     AS [Item], 
       'Band History Timeline (' & CStr(MIN([Year From])) & ' - ' & CStr(MAX([Year To])) & ')' AS [Label],
       'Band History Timeline (' & CStr(MIN([Year From])) & ' - ' & CStr(MAX([Year To])) & ')' AS [Tooltip],
       'graphborder_begin'                                                                     AS [Style Name]
       FROM [band$]
```

### Step 3 - Create continuous year backbone

Generate a **continuous year backbone** using `ENUMERATE`. This fills in any gaps between the earliest and latest years in the dataset.

``` sql
SELECT TRUE AS [ENUMERATE], MIN([Year From]) AS [START AT], MAX([Year To]) AS [STOP AT], 1 AS [STEP BY], 
       '{step}'    AS [Item], 
       'edge_year' AS [Style Name],
       TRUE        AS [CREATE EDGES]
FROM   [band$]
WHERE  IsNumeric([Year From])
```

### Step 4 - Style year nodes

Apply consistent styling to each year node, separating the structural backbone from the band nodes.

``` sql
SELECT TRUE AS [ENUMERATE], MIN([Year From]) AS [START AT], MAX([Year To]) AS [STOP AT], 1 AS [STEP BY], 
       '{step}'    AS [Item], 
       'edge_year' AS [Style Name],
       TRUE        AS [CREATE EDGES]
FROM   [band$]
WHERE  IsNumeric([Year From])
```

### Step 5 - Draw start–finish edges for each band

Create edges representing each band’s active period. 

These edges:
- Run from the start year to the end year  
- Carry genre‑based styling  
- Include tooltips with descriptive metadata  

``` sql
SELECT [Band] & ' ' & [Year From]                 AS [Item], 
       [Year From]                                AS [Tail Label],
       [Band] & ' ' & [Year To]                   AS [Related Item],
       [Year To]                                  AS [Head Label],
       REPLACE('edge_genre_' & [Genre], ' ', '_') AS [Style Name],
       [Band] & ' | ' & [Genre] & ' ' & [Band Type] & ' | ' & [Origin] & ' | ' & [Years Active] & Chr(10) & Chr(10) & 
       [Description]                              AS [Tooltip] 
FROM   [band$]
WHERE  [Year From] IS NOT NULL AND [Year To] IS NOT NULL AND [Year From] <> [Year To]
AND    [Year To]   IS NOT NULL
ORDER BY [Year From]    desc, 
         [Band] desc
```

### Step 6 – Group items by year using ITERATE

Group all items by year using `ITERATE` paired with `CREATE RANK`. This ensures that:

- Year nodes  
- Bands starting that year  
- Bands ending that year  

all align vertically.

The mechanics behind this grouping are worth calling out because the SQL uses multiple Relationship Visualizer extensions along with a clever `UNION` pattern to keep everything aligned:

> [!TIP]
> To group items around each year, showing the year itself, bands that started that year, and bands that ended that year, the timeline uses an `ITERATE` query extension. An `ITERATE` query accepts two SQL statements as parameters:
>
> - The **SQL FOR ID** statement consolidates the list of years by UNION‑ing the distinct values from the *Year From* and *Year To* columns.  
> - The **SQL FOR DATA** statement runs once for each record returned by SQL FOR ID. In this example, we use a clever `UNION` pattern across four SELECT queries to capture all cases (Year From, Year To, Band Year From, Band Year To). Duplicate results are removed by the `UNION`. The SQL also includes the `CREATE RANK` metadata keyword with a value of `'same'`, which places all items for that year on the same rank and produces a clean subgroup for that year.
>
> The result is a clean subgroup for every year in the dataset, regardless of whether the year marks a start, an end, or both.

``` sql
SELECT TRUE AS [ITERATE],
  'SELECT DISTINCT [Year From] AS [ID] FROM [band$] 
     UNION 
   SELECT DISTINCT [Year To]   AS [ID] FROM [band$]
  ' 
  AS [SQL FOR ID],

  'SELECT DISTINCT [Year From]         AS [ITEM], TRUE AS [CREATE RANK], ''same'' AS [RANK] FROM [band$] WHERE [Year From] = {ID}
     UNION
   SELECT DISTINCT [Year To]           AS [ITEM], TRUE AS [CREATE RANK], ''same'' AS [RANK] FROM [band$] WHERE [Year To]   = {ID}
     UNION
   SELECT [Band] & '' '' & [Year From] AS [ITEM], TRUE AS [CREATE RANK], ''same'' AS [RANK] FROM [band$] WHERE [Year From] = {ID}  
     UNION
   SELECT [Band] & '' '' & [Year To]   AS [ITEM], TRUE AS [CREATE RANK], ''same'' AS [RANK] FROM [band$] WHERE [Year To]   = {ID}    
  '
  AS [SQL FOR DATA]
```
### Step 7 - Apply genre‑based node styling

Apply genre‑based styling to band nodes, using color and shape to distinguish categories.

``` sql
SELECT [Band] & ' ' & [Year To]                                    AS [Item], 
       '\r' & [Band] & '\r' & [Year From] & '-' & [Year To] & '\r' AS [External Label], 
       REPLACE('node_genre_' & [Genre], ' ', '_' )                 AS [Style Name],  
       '""'                                                        AS [Label],
       [Band] & ' | ' & [Genre] & ' ' & [Band Type] & ' | ' & [Origin] & ' | ' & [Years Active] & Chr(10) & Chr(10) & 
       [Description]                                               AS [Tooltip]
FROM   [band$]

SELECT [Band] & ' ' & [Year From]                                  AS [Item], 
       [Band] & '\r' & [Year From] & '-' & [Year To]  & '\r'       AS [External Label], 
       REPLACE('node_genre_' & [Genre], ' ', '_' )                 AS [Style Name], 
       '""'                                                        AS [Label],
       [Band] & ' | ' & [Genre] & ' ' & [Band Type] & ' | ' & [Origin] & ' | ' & [Years Active] & Chr(10) & Chr(10) & 
       [Description]                                               AS [Tooltip]   
FROM   [band$]
```

### Step 8 - Close the border around the graph

Close the surrounding cluster which provides the graph title.

``` sql
SELECT '}'               AS [Item], 
       'graphborder_end' AS [Style Name]
```

## Visualize the Results
`PREVIEW AS UNDIRECTED GRAPH` runs Graphviz’s `dot` command and displays the timeline on the `graph` worksheet without arrowheads.

`PUBLISH AS UNDIRECTED GRAPH` runs Graphviz’s `dot` command and saves the generated graph without arrowheads to the current directory.

The resulting diagram reflects the historical activity ranges defined in `musicians.xlsx`.
