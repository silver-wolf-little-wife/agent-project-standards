# agent-project-standards

A universal conventions guide for AI agents and developers collaborating on GitHub open-source projects.

## What Is This?

This is a comprehensive, project-agnostic set of standards that any AI coding agent (or human developer) can follow when working on GitHub open-source projects. It covers the full collaboration lifecycle: from searching for existing project conventions, to writing code, creating PRs, verifying ownership, and publishing releases.

## Key Features

- **Convention-first approach** — mandates searching for and following the project's own style guides before any code change
- **Language auto-detection** — adapts to the project's collaboration language instead of hardcoding one
- **Ownership verification** — prevents accidental merges/releases on repositories you don't own
- **Multi-ecosystem support** — covers JS/TS, Python, Rust, Go, Ruby, Java/Kotlin, Swift, C/C++ and more
- **9 common scenarios** — fork workflows, hotfixes, monorepos, dependency updates, CI/CD changes, and more
- **Multi-shell compatible** — provides correct CLI text handling for Bash, Zsh, Fish, and PowerShell
- **License guide** — covers MIT, Apache 2.0, BSD, GPL-3.0 and other common open-source licenses

## Sections

| # | Section | Description |
|---|---------|-------------|
| 0 | Project Convention File Search | Mandatory first step before any code change |
| 1 | GitHub Collaboration Standards | Language specs + behavior specs for open-source |
| 2 | Shell CLI Multi-line Text Handling | Correct multi-line text in `gh` commands across shells |
| 3 | Release Workflow | Version bump, PR, ownership check, merge, release |
| 4 | Open-Source License Guide | Choosing and writing licenses for open-source projects |
| 5 | Common Development Scenarios | Fork, hotfix, monorepo, deps, CI/CD, refactoring, etc. |
| 6 | Agent Configuration Reference | Auto-detection checklist and project profile template |

## Usage

### For AI Agents

Load `SKILL.md` as a skill/prompt context. The agent will automatically:

1. Search for project convention files before making changes
2. Detect the project's collaboration language and versioning strategy
3. Follow Conventional Commits and minimal change principles
4. Verify repository ownership before merge/release
5. Adapt to the project's code style, naming, and tooling

### For Developers

Read `SKILL.md` as a reference guide for:

- Writing good PRs and commit messages
- Contributing to open-source projects (especially forks)
- Setting up a new open-source project's conventions
- Understanding release workflows and license choices

### For QoderWork

Save the `.skill` file to install as a QoderWork skill. It will be automatically invoked when making PRs, releases, or code changes.

## File Structure

```
agent-project-standards/
├── SKILL.md        # The main conventions document (all sections)
├── README.md       # This file
├── LICENSE         # MIT License
└── .gitignore      # Git ignore rules
```

## Contributing

Contributions are welcome! Please:

1. Fork the repository
2. Create a feature branch (`feat/description`)
3. Follow Conventional Commits format
4. Create a PR with a clear description

## License

MIT License — see [LICENSE](LICENSE) for details.
