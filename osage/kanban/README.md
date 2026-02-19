# Kanban Boards

## Overview

A **Kanban Board** is a visual workflow management system that organizes work into columns—typically **To Do**, **Doing**, and **Done**—to make task status, flow, and bottlenecks immediately visible. Kanban emphasizes limiting work in progress, improving flow, and enabling teams to see the state of their work at a glance. 

This package demonstrates how to model and visualize Kanban Boards using the Relationship Visualizer with SQL‑driven queries. It includes a curated dataset of tasks, statuses, work types, and importance levels, along with a clean, minimal color‑coding scheme designed for clarity and fast visual scanning.

For a concise introduction to Kanban principles, see the [Kanban Guide](https://kanban.university/guide/) by Kanban University.

| ![](./Graph%20-%20All%20Styles.png) |
| :------------------------: |

*Example Kanban board rendered with pastel work‑type colors*

The included workbook, `kanban.xlsx`, provides a realistic set of 50 tasks suitable for demonstrating Kanban workflows. Each task includes a unique ID, short name, brief description, status, work type, and importance. SQL statements transform these flat records into a structured Kanban board, grouping tasks by status and applying consistent color and border styling based on work type.

The sample includes enough records to show that the Relationship Visualizer can handle substantial task lists without difficulty.

## Quick Start

To generate a Kanban board:

1. Open the **Relationship Visualizer** workbook.  
2. Navigate to the **SQL** worksheet.  
3. Ensure the **DATA FILE** points to `Kanban Data.xlsx`.  
4. Click **Run SQL Commands**.  
5. The Kanban board will appear on the **graph** worksheet.  
6. Use the **Graphviz ribbon tab** to adjust layout, spacing, and visual settings.

This is the fastest way to explore the dataset.

## Color‑Coding Scheme

The Kanban example uses a **minimal, pastel‑based color palette** that mimics Post-it® Notes to distinguish work types. Each work type has a light fill color for readability and a deeper border color for contrast.

### Work Types and Colors

| Work Type | Fill Color | Border Color |
|-----------|------------|--------------|
| Feature Work | `#FFF4A3` | `#C2A600` |
| Bug Fix | `#F7B7C9` | `#B23A55` |
| Research / Spike | `#A9D3F5` | `#2A6FA1` |
| Customer Requests | `#C7EFC4` | `#3F8F3A` |
| Dependency / Blocker | `#F9C89B` | `#C46A1B` |

This palette is intentionally restrained:

- Five hue families keep the board readable.  
- Pastel fills ensure handwriting and labels remain legible.  
- Deep borders provide structure without overwhelming the layout.  
- Warm colors signal friction (bugs, blockers, dependencies).  
- Cool colors signal exploration and customer value.

## Data Used

This dataset for the **Relationship Visualizer** tool models a realistic Kanban board, focusing on tasks, work types, statuses, and importance levels. The Excel spreadsheet includes:

- **50 tasks** with unique IDs  
- **Four statuses:** To Do, Doing, Done, Archive  
- **Five work types:** Feature Work, Bug Fix, Research/Spike, Customer Requests, Dependency/Blocker  
- **Three importance levels:** High, Medium, Low  

The dataset is designed to demonstrate:

- How to group tasks by status  
- How to apply color‑coding based on work type  
- How to filter tasks using Views  
- How to visualize workflow at scale  

## Summary

The Kanban Boards example brings together SQL‑driven modeling, style‑based Views, and a minimal color‑coding scheme to produce a clear and navigable visualization of a real‑world workflow. It shows how the Relationship Visualizer can organize complex task lists, highlight structure through color and grouping, and present a clean, readable board even at larger scales. This example provides a practical foundation for building your own Kanban‑style visualizations with similar patterns and techniques.