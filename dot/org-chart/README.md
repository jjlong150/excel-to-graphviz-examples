# Organization Charts

## Overview
This package demonstrates how to generate organization charts using the Relationship Visualizer with SQL‑driven queries. The included workbook, `employee-data.xlsx`, provides a clean, neutralized employee dataset suitable for hierarchical modeling, reporting, and visualization.

The example SQL statements show how to transform flat employee records into parent–child relationships that the Relationship Visualizer can render as a graph.

![](./Graph%20-%20All%20Styles.svg)

This sample contains **test data only**. No real employee information is included.

## Quick Start

If you want to generate an organization chart immediately, follow these steps:

1. Open the **Relationship Visualizer** workbook.  
2. Go to the **SQL** worksheet.  
3. In columns `G–H`, enter:
   - An **Employee ID Number** (use `EID‑0694` for the full enterprise tree)  
   - The number of **levels below** to include  
   - The number of **levels above** to include  
   - TRUE/FALSE for whether to **Include Associates**  
4. Click **Run SQL Commands**.  
5. After a few seconds, the org chart will appear on the **graph** worksheet.  
6. Use the **Graphviz ribbon tab** to adjust layout, arrowheads, zoom, and other visual settings.

This is the fastest way to explore the hierarchy defined in `employee-data.xlsx`.

## Workbook Structure

### employee-data.xlsx
The workbook contains a single table with the following columns:

- Employee ID  
- Employee Name  
- Job Title  
- Supervisor ID  
- Supervisor Name  
- Department Name  
- Role Tier  
- Is People Leader  

The workbook includes **1092 synthetic records** designed to exercise hierarchy depth, branching, and cross‑functional structures.

## Data Dictionary

The `employee-data.xlsx` workbook contains a single normalized `Employee` table. 

The structure of the `Employee` table mirrors the type of flat, row‑based exports commonly produced by standard HR systems such as PeopleSoft, Workday, and SAP HCM.

Each column is described below.

### Employee Table Columns

| Column Name        | Description |
|--------------------|-------------|
| **Employee ID**    | Unique identifier for each employee (`EID‑####` format). |
| **Employee Name**  | Display name used in the org chart. |
| **Job Title**      | Employee job title |
| **Supervisor ID**  | Employee ID of the person’s direct supervisor. |
| **Supervisor Name**| The name of the employee's supervisor, paired to the Supervisor ID. |
| **Department Name**| Department assignment of the employee. |
| **Role Tier**      | Hierarchical tier label (President, EVP, VP, Director, etc.). |
| **Is People Leader** | TRUE/FALSE flag indicating whether the employee manages others. |

### Role Tier Reference (org-levels worksheet)

| Field | Description |
|-------|-------------|
| **Role Rank** | Numeric value used to sort the hierarchy from top to bottom. |
| **Role Tier** | Name of the tier (President, EVP, SVP, etc.). |
| **Employee IDs** | IDs of employees belonging to that tier; collected here as test data to be used as parameters in SQL queries. |

These fields support recursive traversal, parameterized filtering, and consistent hierarchical rendering in the Relationship Visualizer.

## Role Tier Hierarchy
Role Tiers are ranked using a controlled vocabulary to ensure a consistent organizational tree structure.

| Role Rank | Role Tier |
|-----------|-----------|
| 1 | President |
| 2 | Executive Vice President |
| 3 | Associate General Counsel |
| 3 | Senior Vice President |
| 4 | Vice President |
| 5 | Director |
| 6 | Manager |
| 7 | Supervisor |
| 8 | Associate |

The Employee ID values associated with each Role Tier are listed in the `org-levels` worksheet.  
These IDs can be inserted into the parameter fields on the Relationship Visualizer SQL worksheet to generate visualizations for specific organizational subsets.

## SQL Queries for Relationship Visualizer

The Relationship Visualizer supports recursive SQL queries that can traverse a hierarchy and return **From / To** relationships.

Changing parameters in the Relationship Visualizer workbook allows you to:

- Set the starting Employee ID for the org chart  
- Generate charts for a single department  
- Include 0–n departments above the selected department  
- Include 0–n departments below the selected department  
- Toggle inclusion of Team Associates (employees who are not people leaders)  
- Highlight people leaders  
- Visualize only the top tiers (President → EVP → SVP → VP)  

These queries can be modified or extended to match your organization’s structure.

## Techniques Used to Create the Org Chart

This organization chart demonstration uses the following techniques:

- Standard SQL queries to create nodes and edges  
- Placeholder substitution for dynamic parameter values  
- Recursive `TREE QUERY` statements to search the hierarchy upward and downward, generating both edges and nodes  
- `ITERATION` with `CONCATENATION` queries to consolidate all Associates on a team into a single node  
- Orphan node elimination using the Graphviz Ribbon Tab option  
- Distinct node styles for the employee’s department, departments above and below the employee, and Associates  
- Distinct edge styles for “Leader to Leader” and “Leader to Associate” relationships  
- Top‑to‑bottom layout rendered as an undirected graph using Graphviz Ribbon Tab options

## How the SQL Builds the Organization Chart

The Relationship Visualizer constructs the org chart using a four‑step process:

    Selected Employee
            │
            ▼
    [Step 1] Create a node for selected employee
            │
            ▼
    [Step 2] Build tree BELOW (Down)
            │
            ├── Recursive TREE QUERY → "Leader to Leader" edges
            └── Recursive TREE QUERY → "Department Below" nodes
            ▼
    [Step 3] Build tree ABOVE (Up)
            │
            ├── Recursive TREE QUERY → "Leader to Leader" edges
            └── Recursive TREE QUERY (with INNER JOIN) → "Department Above" nodes
            ▼
    [Step 4] Add Associates
            │
            ├── ITERATE + CONCATENATE → "Associate" nodes
            └── TREE QUERY → "Leader to Associate" edges
            ▼
    Final Graph (Preview / Publish)

## Steps to Build the Organization Chart

The SQL queries are easier to understand when the process is broken into four steps:

1. Represent the selected employee  
2. Represent the tree above the employee  
3. Represent the tree below the employee  
4. Represent the Associates  

### Step 1
Create a node for the specified employee using the **"Department"** style.

### Step 2
Create the tree **below** the specified employee (Down).

1. Use `TREE QUERY` to recursively search the hierarchy and create edge relationships, applying the **"Leader to Leader"** style.  
2. Run a slightly modified `TREE QUERY` to create the nodes referenced by those edges. Apply the **"Department Below"** style to these nodes.

### Step 3
Create the tree **above** the specified employee (Up).

1. Use `TREE QUERY` to recursively search upward, generating edge relationships and applying the **"Leader to Leader"** style.  
2. Run a modified `TREE QUERY` to create the nodes referenced by those edges. Because of the parent/child reversal, an INNER JOIN is used to obtain the Employee Name. Apply the **"Department Above"** style to these nodes.

### Step 4
Create the nodes for the Associates.

1. Use `ITERATE` with `CONCATENATE` to loop through all distinct leadership Employee IDs.  
   For each ID, select all Associate names and concatenate them into a single node label.  
   Apply the **"Associate"** style to these nodes.  

   **NOTE:** `TREE QUERY` cannot be combined with `ITERATE` and `CONCATENATE`, so nodes are created for every leader. Standard Relationship Visualizer orphan elimination removes nodes not associated with an edge.

2. Use `TREE QUERY` to recursively search the lower portion of the hierarchy and generate edges from leaders to Associates. Apply the **"Leader to Associate"** style to these edges.

### Visualize the Results

`PREVIEW` runs Graphviz’s `dot` command and displays the organization chart on the `graph` worksheet.

`Publish` runs Graphviz’s `dot` command and saves the generated graph to the current directory.

## Using the Relationship Visualizer

1. Open the Relationship Visualizer workbook.  
2. Navigate to the SQL worksheet.  
3. Locate the data input parameter cells beginning on row 2 in columns `G–H`.  
   - Enter an **Employee ID Number** (use `EID‑0694` to generate the full tree) beside the `{ID Number}` placeholder.  
   - Enter the **number of levels below** the employee to display beside the `{Max Down}` placeholder.  
   - Enter the **number of levels above** the employee to display beside the `{Max Up}` placeholder.  
   - Enter TRUE or FALSE beside the `{Include Associates}` placeholder to toggle the display of Associate‑level team members who report to a leader.  
4. Press the `Run SQL Commands` button.  
5. Within approximately five seconds—depending on the scope of the tree—the organization chart will appear in the `graph` worksheet.  
6. Fine‑tune the graph using controls on the Graphviz ribbon tab to adjust layout direction (left‑to‑right vs. top‑down), toggle arrowheads, modify zoom levels, and more.

The resulting diagram will reflect the hierarchy defined in `employee-data.xlsx`.

## Notes

- All data is fictional and intended for demonstration only.  
- Department names and job titles are aggressively neutralized to support generic modeling. There may be branches in the tree that do not reflect a real business structure.  
- The structure reflects a two‑tier executive model:  
  - President at the top  
  - CEO, CHRO, General Counsel reporting to the President  
  - CFO, CIO, CMO, COO reporting to the CEO  

This data provides a realistic but fully synthetic hierarchy for testing.