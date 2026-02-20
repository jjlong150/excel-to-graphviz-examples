# Continents by Area

## Overview
This example demonstrates how to use the **Graphviz patchwork layout** to visualize aggregated values—in this case, the total land area of each continent. Patchwork is a space‑filling layout that sizes each region proportionally, making it ideal for comparing magnitudes at a glance.

The data comes from a workbook named **countries.xlsx**, with a worksheet also named **countries**. Each row represents a country and includes its continent and land area in square kilometers. A simple SQL query groups countries by continent and sums their areas. The resulting dataset is then rendered as a patchwork diagram, with each continent assigned a distinct visual style.

| ![](./Graph%20-%20All%20Styles.png) |
| :--------------------------: |

*Example patchwork diagram showing continents sized by total area*

## Data Source
The workbook `countries.xlsx` contains a worksheet named **countries** with the following relevant fields:

- **Continent** — continent name
- **AreaInSquareKm** — land area of the country in square kilometers

Each row corresponds to a single country.

## SQL Query
The Relationship Visualizer uses the following SQL query to aggregate total area by continent:

```sql
SELECT   [Continent]                               AS [Item], 
         [Continent] & '\n' & FORMAT(SUM([AreaInSquareKm]) / 10000000, "0.0") & ' M km²' 
                                                   AS [Label], 
         'area=' & (SUM([AreaInSquareKm])/1000000) AS [Attributes], 
         [Continent]                               AS [Style Name]
FROM     [countries$] 
WHERE    [AreaInSquareKm] IS NOT NULL
GROUP BY [Continent] 
ORDER BY [Continent]

```

This produces one row per continent, with the summed area of all countries belonging to that continent.

### Design Considerations and Current Limitations

The **patchwork** layout is optimized for proportional, space‑filling diagrams and therefore omits several structural cues that other Graphviz layouts provide. Patchwork does not render edges, and it offers no built‑in visual indicators for hierarchy or nesting such as inset rectangles, stepped borders, or heavier outlines. In addition, patchwork cannot reliably place or size cluster labels within the rectangles it generates, since the layout engine does not expose any mechanism for fitting text to the allocated regions.

## Styles

Each continent is assigned a unique style definition to ensure clear visual distinction in the patchwork diagram. Styles include a **fill color**, **border color**, and **filled** rendering.

### Continent Styles
<table>
  <thead>
    <tr>
      <th>Continent</th>
      <th>Fill</th>
      <th>Border</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Africa</td>
      <td style="background-color:#FAF0E1;">#FAF0E1</td>
      <td style="background-color:#A8783C; color:white;">#A8783C</td>
    </tr>
    <tr>
      <td>Asia</td>
      <td style="background-color:#EBF5FA;">#EBF5FA</td>
      <td style="background-color:#467896; color:white;">#467896</td>
    </tr>
    <tr>
      <td>Europe</td>
      <td style="background-color:#F5F5EB;">#F5F5EB</td>
      <td style="background-color:#82825A; color:white;">#82825A</td>
    </tr>
    <tr>
      <td>North America</td>
      <td style="background-color:#EBFAF0;">#EBFAF0</td>
      <td style="background-color:#468C64; color:white;">#468C64</td>
    </tr>
    <tr>
      <td>South America</td>
      <td style="background-color:#FAEBF0;">#FAEBF0</td>
      <td style="background-color:#96506E; color:white;">#96506E</td>
    </tr>
    <tr>
      <td>Oceania</td>
      <td style="background-color:#EBF5F0;">#EBF5F0</td>
      <td style="background-color:#508278; color:white;">#508278</td>
    </tr>
    <tr>
      <td>Antarctica</td>
      <td style="background-color:#F5FAFF;">#F5FAFF</td>
      <td style="background-color:#7896AA; color:white;">#7896AA</td>
    </tr>
  </tbody>
</table>

These styles are referenced automatically when the graph is generated.

## Graphviz Output
The **patchwork** layout engine produces a proportional diagram where each continent occupies an area sized according to its total land area. Larger continents receive larger rectangles, making relative scale immediately visible.

The Relationship Visualizer automatically applies the continent styles and generates a `.gv` file and rendered output.

## Files Included
- `countries.xlsx` — source data
- `Relationship Visualizer.xlsm` — tool used to run SQL and generate the graph


## Summary
This example illustrates how to:

- Query worksheet data using SQL
- Aggregate values with `GROUP BY`
- Apply continent‑specific styles
- Use the Graphviz **patchwork** layout to create a proportional area diagram
