# Excel to Graphviz Examples

**Transform Excel data into professional Graphviz diagrams effortlessly.**

This repository is your go-to collection of practical, ready-to-use examples for the **Relationship Visualizer** — the powerful Excel spreadsheet that turns tabular data into stunning Graphviz visualizations.

Whether you're mapping org charts, dependencies, hierarchies, business processes, data classifications, or complex networks, these examples show real-world patterns using SQL queries, styling tricks, and every major Graphviz layout engine.

## Quick Links

[![Latest Release v10.1.0](https://img.shields.io/badge/Latest%20Release-v10.1.0%20(Feb%202026)-brightgreen)](https://github.com/jjlong150/ExcelToGraphviz/releases/latest)
[![Download Now](https://img.shields.io/badge/Download%20Now-Free%20Excel%20Tool-green)](https://sourceforge.net/projects/relationship-visualizer/files/latest/download)
[![Changelog](https://img.shields.io/badge/Full%20Changelog-exceltographviz.com/changelog-blue)](https://exceltographviz.com/changelog)

- 🌐 **Main Website & Documentation**: [exceltographviz.com](https://exceltographviz.com/) — tutorials, guides, and more
- 📥 **Latest Release (v10.1.0 — Feb 2026)**: [GitHub Releases](https://github.com/jjlong150/ExcelToGraphviz/releases/latest) — what's new & download assets
- ⬇️ **Download Relationship Visualizer**: [SourceForge (Latest ZIP)](https://sourceforge.net/projects/relationship-visualizer/files/latest/download) — free tool, ~81 MB
- 📜 **Full Changelog**: [exceltographviz.com/changelog](https://exceltographviz.com/changelog) — detailed version history
- 🔧 **Core Tool Repository**: [github.com/jjlong150/ExcelToGraphviz](https://github.com/jjlong150/ExcelToGraphviz) — source, issues, and contributions
- ☕ **Support / Buy Me a Coffee**: [buymeacoffee.com/exceltographviz](https://www.buymeacoffee.com/exceltographviz) — optional appreciation if this tool helps your work!

## Browse Examples by Graphviz Layout Engine

Find the perfect layout style for your diagram in seconds:

| Engine     | Best For                                      | Folder                          |
|------------|-----------------------------------------------|---------------------------------|
| **dot**    | Trees, hierarchies, org charts, dependencies  | [`./dot/`](dot/)                |
| **neato**  | Organic, force-directed networks              | [`./neato/`](neato/)            |
| **fdp**    | Medium-sized undirected graphs                | [`./fdp/`](fdp/)                |
| **sfdp**   | Large-scale networks & force layouts          | [`./sfdp/`](sfdp/)              |
| **twopi**  | Radial, concentric, layered diagrams          | [`./twopi/`](twopi/)            |
| **circo**  | Circular, ring, cyclic structures             | [`./circo/`](circo/)            |
| **osage**  | Clustered, partitioned, grouped layouts       | [`./osage/`](osage/)            |
| **patchwork** | Treemap-style, space-filling hierarchies   | [`./patchwork/`](patchwork/)    |

Each engine folder includes:
- Ready-to-open **Excel workbooks** (.xlsx)
- Scenario description in a mini-README
- SQL patterns (including recursive TREE QUERY examples)
- Generated **Graphviz output** (SVG, PNG)
- Styling, layout tweaks, and modeling notes

## Example Previews

Here's a quick taste of what these examples produce:

![Org Chart Example (dot engine)](./dot/org-chart/Graph%20-%20All%20Styles.png)  
*Hierarchical org chart using dot layout*

![Network Example (neato)](./neato/rock-band-musician-connections/Graph%20-%20All%20Styles.png)  
*Force-directed network using neato*

## What You'll Discover in These Examples

- Real SQL queries powering node/edge generation (enhanced in v10.1.0 with concatenation modes for cleaner detail aggregation)
- Turning flat Excel tables (HR exports, BOMs, process lists, etc.) into rich, dynamic graphs
- Recursive hierarchies, multi-level dependencies, filtered/parameterized views
- Advanced node & edge styling for maximum clarity and visual impact
- Best practices for Excel-to-Graphviz data modeling workflows
- Techniques inspired by enterprise exports (all datasets are **synthetic/fictional**)

These aren't just pretty pictures — they're reusable templates that teach you how to:

- Model complex relationships directly in Excel
- Leverage SQL + Graphviz for dynamic, data-driven diagrams
- Avoid common pitfalls in graph layout and readability

## Who Uses This?

Analysts, data modelers, architects, process engineers, HR teams, IT documentation specialists — anyone who needs to **visualize relationships**, dependencies, or structures that live in spreadsheets.

## Contributing

New examples, better SQL patterns, styling improvements, or additional engines? Pull requests are welcome! See [CONTRIBUTING.md](./CONTRIBUTING.md) for guidelines."

If you've built an interesting diagram type or technique with Relationship Visualizer, please share it here — it'll help the whole community.

## License

[MIT License](./LICENSE) — free to use, modify, and share.

Start exploring the engine folders above, download the latest v10.1.0 from [exceltographviz.com](https://exceltographviz.com/), or jump straight to the [core repo](https://github.com/jjlong150/ExcelToGraphviz) to get diagramming today!