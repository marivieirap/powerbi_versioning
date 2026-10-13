# Power BI Versioning with Git

Learning Git version control with Power BI through a simple calendar project.

## About the Project

This repository demonstrates how to use Git for version control in a Power BI project using the **Power BI Project format (`.pbip`)**.

The project uses a calendar report to explore how Git can track changes to report definitions and the semantic model, maintain a commit history, compare versions, and restore previous states.

## Project Structure

```text
powerbi_versioning/
├── Calendario.Report/
│   └── definition.pbir
├── Calendario.SemanticModel/
├── .gitignore
├── Calendario.pbip
└── README.md
```

- **`Calendario.pbip`**: Entry point for the Power BI project.
- **`Calendario.Report/`**: Contains the report definition, including pages and visual configurations.
- **`Calendario.SemanticModel/`**: Contains the semantic model definition and related metadata.
- **`.gitignore`**: Excludes unnecessary local files and `.pbix` files from version control.

## Getting Started

### Prerequisites

- [Power BI Desktop](https://powerbi.microsoft.com/desktop/)
- [Git](https://git-scm.com/)
- A Git client or terminal

### Clone the Repository

```bash
git clone https://github.com/marivieirap/powerbi_versioning.git
cd powerbi_versioning
```

Alternatively, download the repository as a ZIP file from GitHub and extract it.

### Open the Power BI Report

1. Navigate to the `Calendario.Report/` folder.
2. Open the `definition.pbir` file using Power BI Desktop.
3. Explore the report pages, filters, visualizations, and interactions.

**Important:** Keep the original folder structure intact so the report can reference its associated semantic model correctly.

GitHub does not render the interactive Power BI report directly. Open the project in Power BI Desktop to explore its visualizations.

## Git Versioning

The commit history documents the project's evolution, from the initial calendar model to report improvements and experiments with visual themes.

The repository includes examples of:

- Organizing commits to track project changes.
- Reviewing differences between versions with `git diff`.
- Restoring tracked files from a previous commit with `git restore`.
- Understanding the differences between `git restore`, `git revert`, and `git reset`.
- Recording a restoration as a new commit instead of rewriting the existing history.

Each commit provides a snapshot of the project's tracked files, making it easier to understand and review changes over time.

## Documentation

Detailed documentation is available in both languages:

- 🇧🇷 [Português (Brasil) — Guia de versionamento com Git](docs/versionamento-git-pt-br.md)
- 🇺🇸 [English (US) — Git Versioning Guide](docs/git-versioning-en.md)

## References

- [Microsoft Learn — Power BI Projects (PBIP)](https://learn.microsoft.com/en-us/power-bi/developer/projects/projects-overview)
- [Git Documentation](https://git-scm.com/doc)

## Disclaimer

This repository is a learning project created to explore Git version control practices in Power BI. It is not intended to represent a production-ready deployment workflow.