# agent-project-standards

面向 AI agent 和开发者的通用 GitHub 开源项目协作规范。

A universal conventions guide for AI agents and developers collaborating on GitHub open-source projects.

---

## 这是什么？ / What Is This?

这是一套全面的、项目无关的协作规范，任何 AI 编码 agent（或人类开发者）在参与 GitHub 开源项目时都可以遵循。它覆盖了完整的协作生命周期：从搜索项目现有规范、编写代码、创建 PR、验证所属权，到发布 Release。

This is a comprehensive, project-agnostic set of standards that any AI coding agent (or human developer) can follow when working on GitHub open-source projects. It covers the full collaboration lifecycle: from searching for existing project conventions, to writing code, creating PRs, verifying ownership, and publishing releases.

## 核心特性 / Key Features

- **规范优先** — 在任何代码修改前强制搜索并遵循项目自身的风格指南
- **Convention-first** — mandates searching for and following the project's own style guides before any code change

- **语言自动检测** — 适配项目的协作语言，而非硬编码某一种
- **Language auto-detection** — adapts to the project's collaboration language instead of hardcoding one

- **所属权验证** — 防止在他人仓库上误执行合并/发布操作
- **Ownership verification** — prevents accidental merges/releases on repositories you don't own

- **多生态系统支持** — 覆盖 JS/TS、Python、Rust、Go、Ruby、Java/Kotlin、Swift、C/C++ 等
- **Multi-ecosystem support** — covers JS/TS, Python, Rust, Go, Ruby, Java/Kotlin, Swift, C/C++ and more

- **10 种常见场景** — Fork 工作流、Hotfix、Monorepo、依赖更新、CI/CD 变更、发布新项目等
- **10 common scenarios** — fork workflows, hotfixes, monorepos, dependency updates, CI/CD changes, publishing new projects, and more

- **多 Shell 兼容** — 为 Bash、Zsh、Fish、PowerShell 提供正确的 CLI 多行文本处理
- **Multi-shell compatible** — provides correct CLI text handling for Bash, Zsh, Fish, and PowerShell

- **许可证指南** — 覆盖 MIT、Apache 2.0、BSD、GPL-3.0 等常见开源许可证
- **License guide** — covers MIT, Apache 2.0, BSD, GPL-3.0 and other common open-source licenses

## 章节概览 / Sections

| # | 章节 / Section | 描述 / Description |
|---|----------------|---------------------|
| 0 | 项目规范文件搜索 / Convention File Search | 代码修改前的强制前置步骤 / Mandatory first step before any code change |
| 1 | GitHub 协作标准 / Collaboration Standards | 语言规范 + 行为规范 / Language specs + behavior specs |
| 2 | Shell CLI 多行文本处理 / CLI Text Handling | 跨 Shell 的正确多行文本处理 / Correct multi-line text across shells |
| 3 | 发版流程 / Release Workflow | 版本递增、PR、所属权核对、合并、发布 / Version bump, PR, ownership check, merge, release |
| 4 | 许可证指南 / License Guide | 选择和编写开源许可证 / Choosing and writing open-source licenses |
| 5 | 常见开发场景 / Common Scenarios | Fork、Hotfix、Monorepo、依赖更新、CI/CD、发布新项目等 / Fork, hotfix, monorepo, deps, CI/CD, new project, etc. |
| 6 | Agent 配置参考 / Agent Config Reference | 自动检测清单与项目画像模板 / Auto-detection checklist and project profile template |

## 使用方法 / Usage

### 面向 AI Agent / For AI Agents

将 `SKILL.md` 作为 skill 或 prompt 上下文加载。Agent 会自动：

Load `SKILL.md` as a skill/prompt context. The agent will automatically:

1. 在修改前搜索项目规范文件 / Search for project convention files before making changes
2. 检测项目的协作语言和版本策略 / Detect the project's collaboration language and versioning strategy
3. 遵循 Conventional Commits 和最小变更原则 / Follow Conventional Commits and minimal change principles
4. 在合并/发布前验证仓库所属权 / Verify repository ownership before merge/release
5. 适配项目的代码风格、命名和工具链 / Adapt to the project's code style, naming, and tooling

### 面向开发者 / For Developers

将 `SKILL.md` 作为参考指南，用于：

Read `SKILL.md` as a reference guide for:

- 编写高质量的 PR 和 commit message / Writing good PRs and commit messages
- 参与开源项目协作（尤其是 Fork 场景） / Contributing to open-source projects (especially forks)
- 搭建新开源项目的规范体系 / Setting up a new open-source project's conventions
- 理解发版流程和许可证选择 / Understanding release workflows and license choices

### 面向 QoderWork / For QoderWork

保存 `.skill` 文件即可安装为 QoderWork skill，在创建 PR、发布 Release 或修改代码时会自动调用。

Save the `.skill` file to install as a QoderWork skill. It will be automatically invoked when making PRs, releases, or code changes.

## 文件结构 / File Structure

```
agent-project-standards/
├── SKILL.md        # 核心规范文档（全部章节） / Main conventions document (all sections)
├── README.md       # 本文件 / This file
├── LICENSE         # MIT 许可证 / MIT License
└── .gitignore      # Git 忽略规则 / Git ignore rules
```

## 贡献 / Contributing

欢迎贡献！请遵循： / Contributions are welcome! Please:

1. Fork 本仓库 / Fork the repository
2. 创建功能分支（`feat/描述`） / Create a feature branch (`feat/description`)
3. 使用 Conventional Commits 格式 / Follow Conventional Commits format
4. 创建 PR 并附上清晰描述 / Create a PR with a clear description

## 许可证 / License

MIT 许可证 — 详见 [LICENSE](LICENSE) 文件。

MIT License — see [LICENSE](LICENSE) for details.
