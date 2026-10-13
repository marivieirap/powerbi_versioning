# Git Versioning for Power BI Projects

**Author:** Mariana Vieira Paiva  

## 1. Overview

This project demonstrates how to use Git and GitHub to version a Power BI project, using a calendar report as a practical example.

The goal is to track the project's evolution through small, descriptive commits, understand which files changed, and demonstrate how to restore the contents of an earlier version without deleting the more recent history.

The project uses the **Power BI Project (`.pbip`)** format, which stores report and semantic model definitions in a folder structure. This allows Git to track changes at a more granular level than a traditional `.pbix` file.

### Goals

- Learn the basic Git concepts used in a Power BI project.
- Track incremental changes through identifiable commits.
- Compare versions and identify changed files.
- Restore the contents of an earlier version and record the restoration as a new commit.
- Explore the benefits and limitations of versioning Power BI projects.

## 2. Prerequisites

To reproduce this experiment, you will need:

- **Power BI Desktop**, with support for the Power BI Project (`.pbip`) format.
- **Git** installed on your machine.
- A **GitHub** account and a repository to store the project.
- Basic terminal skills and familiarity with `status`, `add`, `commit`, `log`, `diff`, and `restore`.

You can also refer to Microsoft's official documentation on [Power BI projects (PBIP)](https://learn.microsoft.com/power-bi/developer/projects/projects-overview).

## 3. Project structure

The repository's main structure is:

```text
powerbi_versioning/
├── Calendario.Report/
│   └── definition.pbir
├── Calendario.SemanticModel/
├── .gitignore
├── Calendario.pbip
└── README.md
```

The report and semantic model directories contain the project's definitions. The `Calendario.pbip` file in the repository root is the entry point for opening the project in Power BI Desktop.

The `.gitignore` file prevents local and cache files that are not needed for sharing the project from being versioned, including `.pbix` files:

```gitignore
**/.pbi/localSettings.json
**/.pbi/cache.abf
*.pbix
```

The `.pbix` file was removed from the repository in commit `rev006` because it is not required to demonstrate version control with the PBIP project format.

> **Important:** Keep the project's complete folder and file structure. The `Calendario.pbip` file references definitions stored in the report and semantic model directories.

## 4. Initial Git setup and workflow

After installing Git and cloning the repository, the basic workflow for recording a change is:

```bash
git status
git add .
git commit -m "Short description of the change"
git push
```

What each command does:

- `git status`: shows modified, added, or deleted files.
- `git add .`: stages changes under the current directory for the next commit. Review the result before committing.
- `git commit -m "..."`: records a point in the local history.
- `git push`: sends local commits to the remote repository.

To view a concise commit history:

```bash
git log --oneline
```

To inspect changes before committing:

```bash
git diff
```

Small commits, each focused on a logical change, make reviews easier and help identify when a change was introduced.

## 5. Commit organization

The commits in this experiment are labeled sequentially from `rev000` to `rev011` to make the project's evolution easier to follow.

| Commit | Description |
|---|---|
| `rev000` | Initial commit: base calendar model |
| `rev001` | Create README |
| `rev002` | Update the model's `PBI_ProTooling` annotation |
| `rev003` | Set a short date format, sort Month by MesNum, and add Day and Day Name |
| `rev004` | Add a measures table |
| `rev005` | Add `tb_Calendar` and `tb_Feriados` tables and report visuals |
| `rev006` | Remove the PBIX file and update `.gitignore` |
| `rev007` | Update the `HTML_Calendario` measure and add Year and Month filters |
| `rev008` | Create dark mode |
| `rev009` | Set dark mode as the default |
| `rev010` | Set light mode as the default |
| `rev011` | Restore the project to the state of `rev009` |

This sequence shows how model, report, and visual configuration changes can be recorded in separate steps. The `rev000` to `rev011` labels are naming conventions used for this study, not a Git requirement.

## 6. Inspecting changes between versions

The `git log --oneline` command helps identify commits. To see which files were changed in a specific commit:

```bash
git show --stat <commit-hash>
```

To compare report files between two versions, run:

```bash
git diff <previous-hash> <later-hash> -- Calendario.Report/
```

To compare semantic model definitions:

```bash
git diff <previous-hash> <later-hash> -- Calendario.SemanticModel/
```

Replace the placeholders with the actual commit hashes. For example, to investigate the visual change introduced in `rev008`, compare the preceding commit with the hash for `rev008`, targeting `Calendario.Report/`.

Git can display differences in text files that represent project definitions. However, not every Power BI change will appear as an easy-to-interpret diff; readability depends on the file and how its contents are serialized.

## 7. Practical experiment: restoring an earlier version

The goal of this experiment was to restore the project to the state represented by `rev009`, when dark mode was the default, after `rev010` changed the default to light mode.

The command used was:

```bash
git restore --source=9af6adb --staged --worktree .
```

In this command:

- `--source=9af6adb` specifies the commit whose contents will be used as the source.
- `--staged` updates the staging area, also known as the index.
- `--worktree` updates the working tree files.
- `.` applies the operation to paths under the current directory.

**Caution:** This command replaces the current versions of tracked files with the contents from the source version in both the staging area and working tree. Local changes to those files may be overwritten. Before running it, check `git status` and, if needed, back up or commit any changes you want to keep.

The command does not remove untracked files. Therefore, locally created files that are not tracked by Git are not deleted by this operation.

After validating the report in Power BI Desktop, the restoration was recorded as a new commit:

```bash
git add .
git commit -m "rev011 Restaura o projeto para o estado da rev009"
git push
```

The result is that tracked files reflect the state of `rev009`, while the history still preserves the earlier commits—including `rev010`—and records the restoration in `rev011`.

## 8. Why create a new commit?

Instead of rewriting history, this experiment records the restoration as a new commit. This makes the intent explicit and preserves a record of when the files were restored.

Git provides several ways to undo changes, and the right choice depends on the goal:

| Command | Behavior | When to consider it |
|---|---|---|
| `git restore` | Restores file contents from a source, updating the staging area and/or working tree depending on the options used. | When you need to recover file contents from a specific version. |
| `git revert` | Creates a new commit that undoes the changes introduced by another commit. | When you want to undo a commit without removing it from history, especially on shared branches. |
| `git reset` | Moves the branch pointer to another commit and may change the staging area and working tree, depending on the mode used. | When reorganizing local history; use caution on shared branches. |

In this case, `git revert` could also have been an option if the goal were to undo the changes introduced by `rev010`. The `restore` approach was chosen to demonstrate how to recover tracked file contents from a specific commit and record that restoration in a new commit.

The operation used did not remove `rev010` from history or move the branch backward. Instead, it created `rev011` to document the restoration.

## 9. Opening the project

To open the report in Power BI Desktop:

1. Clone or download this repository.
2. Extract the files if you downloaded the ZIP archive.
3. Navigate to the `Calendario.Report/` folder.
4. Open `definition.pbir` using Power BI Desktop to access the report with the latest version of the visuals.
5. Review the report pages, filters, and interactions.

> **Note:** Keep the repository's folder structure intact so the report can reference the associated semantic model correctly.

GitHub lets you browse the versioned files, but it does not execute or render the interactive Power BI report directly on the repository page.

## 10. Key learnings, limitations, and conclusion

This experiment demonstrated that the PBIP format makes it possible to track report and semantic model definitions in Git, helping users compare versions and document changes.

Key learnings:

- Small, descriptive commits make project history easier to understand.
- `git diff` helps identify which files changed between versions.
- You can restore the contents of an earlier version without removing later commits from history.
- A restoration should be validated in Power BI Desktop because a text diff does not replace checking the report's visual and functional behavior.
- Local files, cache files, and data that do not need to be shared should be considered for exclusion through `.gitignore`.

Version control also has limitations. Git does not replace the Power BI publishing process, permission management, access controls for data sources, or backup mechanisms. Some files or changes may also be difficult to review through text diffs alone.

This repository is a practical learning project and does not aim to cover every Power BI collaboration or deployment scenario. Its main purpose is to show how a simple Git workflow can improve traceability and provide more control over a Power BI project's evolution.

## References

- [Microsoft Learn — Power BI projects (PBIP)](https://learn.microsoft.com/power-bi/developer/projects/projects-overview)
- [Git — official documentation](https://git-scm.com/doc)
- [Git — git restore](https://git-scm.com/docs/git-restore)
- [Git — git revert](https://git-scm.com/docs/git-revert)
- [Git — git reset](https://git-scm.com/docs/git-reset)