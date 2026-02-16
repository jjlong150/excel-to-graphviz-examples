# Excel to Graphviz Examples

This repository contains a curated set of example diagrams and datasets that demonstrate how the Relationship Visualizer spreadsheet transforms Excel data into polished Graphviz diagrams.

**Excel to Graphviz** refers to the full project: the Relationship Visualizer spreadsheet, the documentation, the examples, and the main website.

The **Relationship Visualizer** is the spreadsheet itself — the primary tool used to generate Graphviz diagrams from Excel data.

## Excel to Graphviz Website

For documentation, tutorials, and the latest release, visit:

https://exceltographviz.com/

## Graphviz Layout Engines

Browse examples by Graphviz layout engine to quickly find patterns that match the type of diagram you want to create.

| Engine | Description |
|--------|-------------|
| [**dot**](./dot/) | Hierarchical layouts (trees, org charts, dependency chains) |
| [**neato**](./neato/) | Force‑directed layouts for loosely structured networks |
| [**fdp**](./fdp/) | Force‑directed layouts for medium‑sized undirected graphs |
| [**sfdp**](./sfdp/) | Scalable force‑directed layouts for large networks |
| [**twopi**](./twopi/) | Radial layouts with concentric levels |
| [**circo**](./circo/) | Circular and cyclic layouts |
| [**osage**](./osage/) | Clustered or partitioned layouts |
| [**patchwork**](./patchwork/) | Treemap‑style, space‑filling layouts |

Each engine folder contains:

- One or more **example workbooks**  
- A **mini‑README** explaining the scenario  
- SQL and other patterns used in the Relationship Visualizer  
- The resulting Graphviz output (SVG/PNG)  
- Notes on styling, layout, and modeling techniques  

Beyond the engine‑specific examples, this repository also demonstrates a range of modeling and visualization techniques.

## What You’ll Find Here

- Fully‑worked examples using real SQL queries inside the Relationship Visualizer  
- Demonstrations of Relationship Visualizer patterns (including recursive `TREE QUERY` SQL)  
- Techniques for building diagrams from flat tables using SQL  
- Examples of node and edge styling for clearer diagrams  
- Parameterized SQL patterns for filtering, slicing, and reshaping data  
- Best practices for structuring Excel tables for graph modeling  

## Why These Examples Matter

The Relationship Visualizer is capable of far more than simple diagrams — it can model hierarchies, dependencies, classifications, and multi‑level structures directly from Excel.

These examples are designed to:

- Teach the underlying modeling patterns  
- Show how SQL and Graphviz work together  
- Provide reusable templates for your own projects  
- Demonstrate how to turn spreadsheet data into computable graph structures  

## About the Data

Some examples use synthetic datasets modeled after the flat, row‑based exports commonly produced by enterprise HR systems such as PeopleSoft, Workday, and SAP HCM. These datasets are **fictional** and exist solely to demonstrate modeling techniques.

## Contributing

Contributions, improvements, and new examples are welcome.

If you have a pattern, SQL technique, or visualization style that others might benefit from, feel free to open a pull request.

## License

This repository is released under the **MIT License**.

You are free to use, modify, and distribute the examples with minimal restrictions.